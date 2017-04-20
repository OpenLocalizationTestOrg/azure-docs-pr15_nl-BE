<properties
    pageTitle="Azure AD federation compatibility lijst"
    description="Deze pagina heeft een niet-Microsoft-id-providers die kunnen worden gebruikt voor het implementeren van eenmalige aanmelding."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="billmath"/>

# <a name="azure-ad-federation-compatibility-list"></a>Azure AD federation compatibility lijst
Azure Active Directory biedt eenmalige op en beveiliging van access-toepassingen voor Office 365 en andere Microsoft Online services voor hybride en alleen cloud-implementaties verbeterd zonder dat een niet-Microsoft-oplossing. Office 365, net als de meeste on line services van Microsoft, is geïntegreerd met Active Directory Azure voor adreslijstservices, verificatie en machtiging. Azure Active Directory biedt ook eenmalige aanmelding tot duizenden toepassingen SaaS en webtoepassingen op gebouwen. Zie de galerie Azure Active Directory-toepassing voor ondersteunde SaaS-toepassingen.

Voor organisaties die hebben geïnvesteerd in oplossingen voor niet-Microsoft-federation, bevat dit onderwerp richtlijnen voor het configureren van eenmalige aanmelding voor Windows Server Active Directory-gebruikers met Microsoft Online services met behulp van niet-Microsoft-id-providers in de 'Azure Active Directory federation compatibiliteit "onderstaande lijst. 


![](./media/active-directory-aadconnect-federation-compatibility/oxford2.jpg)   
[Oxford computergroep](http://oxfordcomputergroup.com/), een derde partij, namens Microsoft getest deze single sign-on-ervaringen met niet-Microsoft-id-providers tegen een reeks gemeenschappelijke use-cases met Azure Active Directory.

Voor informatie over hoe u de van derden identiteitsprovider hier vermeld kan krijgen, neem contact op met de Oxford computergroep op [idp@oxfordcomputergroup.com](mailto:idp@oxfordcomputergroup.com).

>[AZURE.IMPORTANT] Oxford computergroep alleen de functionaliteit van deze situaties voor één federation getest. Alle tests van de synchronisatie, verificatie met twee factoren, onderdelen van deze situaties voor één enz. heeft geen uitgevoerd door Oxford computergroep.

>Gebruik van aanmelden door alternatieve UPN-ID is ook niet getest in dit programma.



- [Azure Active Directory](#azure-active-directory)
- [Optimale IDM identiteit virtuele Server Federation Services](#optimal-idm-virtual-identity-server-federation-services) 
- [6.11 PingFederate](#pingfederate-611) 
- [7.2 PingFederate](#pingfederate-72) 
- [PingFederate 8.x](#pingfederate-8x)
- [Centrify](#centrify) 
- [IBM Tivoli federatieve id-beheer 6.2.2](#ibm-tivoli-federated-identity-manager-622) 
- [IdP SecureAuth 7.2.0](#secureauth-idp-720) 
- [CA SiteMinder 12.52](#ca-siteminder-1252-sp1-cumulative-release-4) 
- [RadiantOne 3.0 CFS](#radiantone-cfs-30) 
- [Okta](#okta) 
- [OneLogin](#onelogin) 
- [NetIQ Access Manager 4.0.1](#netiq-access-manager-401) 
- [BIG-IP met Access Policy Manager BIG-IP versie 11.3 x: 11.6 x](#big-ip-with-access-policy-manager-big-ip-ver-113x-116x) 
- [Portal van de werkruimte VMware versie 2.1](#vmware-workspace-portal-version-21) 
- [Log & Start 5.3](#signampgo-53) 
- [IceWall Federatie versie 3.0](#icewall-federation-version-30) 
- [CA Secure Cloud](#ca-secure-cloud) 
- [Dell een identiteit wolk Access Manager v7.1](#dell-one-identity-cloud-access-manager-v71) 
- [AuthAnvil één teken op 4.5](#authavil-single-sign-on-45) 

>[AZURE.IMPORTANT] Aangezien die producten van andere leveranciers, biedt Microsoft geen ondersteuning voor de implementatie, configuratie, probleemoplossing, beste praktijken, enz. problemen en vragen met betrekking tot deze id-providers. Voor ondersteuning en vragen met betrekking tot deze identiteitsproviders rechtstreeks contact opnemen met de ondersteunde derden.

>Deze derden identiteitsproviders zijn getest op compatibiliteit met Microsoft cloud services met behulp van WS-Federation en alleen WS-Trust-protocollen. Testen, bevatten het SAML-protocol.

## <a name="azure-active-directory"></a>Azure Active Directory 
Azure Active Directory kunt u gebruikers verifiëren door federeren met Active Directory in uw op gebouwen of zonder een federatieserver in ruimten met behulp van een wachtwoord synchronisatie. 

Dit is de matrix scenario ondersteuning voor deze ervaring: 


| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Web-gebaseerde clients zoals Web Access voor Exchange en SharePoint Online | Ondersteund |Geen|
| Rich client-toepassingen zoals Lync, Office-abonnement, CRM |  Ondersteund |Geen|
| Uitgebreide e-mail clients zoals Outlook en ActiveSync |  Ondersteund |Geen|
|Moderne toepassingen met behulp van ADAL zoals Office 2016| Ondersteund|Geen|

Zie [Active Directory Federation Services (ADFS)](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs) voor meer informatie over het gebruik van Azure Active Directory met AD FS.

Zie voor meer informatie over het gebruik van Azure Active Directory met Password sync [Azure AD verbinden](active-directory-aadconnect.md).


## <a name="optimal-idm-virtual-identity-server-federation-services"></a>Optimale IDM identiteit virtuele Server Federation Services 
Optimale IDM virtuele identiteit Server Federation Services kunt verifiëren van gebruikers die zich in Active Directory's klanten op locatie bevinden.

Dit is het scenario matrix ondersteunen deze ervaring voor eenmalige aanmelding:


| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Web-gebaseerde clients zoals Web Access voor Exchange en SharePoint Online | Ondersteund |Geen|
| Rich client-toepassingen zoals Lync, Office-abonnement, CRM |  Ondersteund |Geïntegreerde Windows-verificatie|
| Uitgebreide e-mail clients zoals Outlook en ActiveSync |  Ondersteund |Voor meer informatie over de toegang van clients, beleid Zie [beperken van toegang tot Office 365 Services is gebaseerd op de locatie van de Client.](https://technet.microsoft.com/library/hh526961.aspx)|



## <a name="pingfederate-611"></a>6.11 PingFederate 

PingFederate 6.11 implementeert de algemeen geaccepteerde norm van WS-Federation identiteit voor een een-op- en kenmerk exchange framework.

Dit is de matrix scenario ondersteuning voor deze ervaring voor eenmalige aanmelding:


| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Web-gebaseerde clients zoals Web Access voor Exchange en SharePoint Online | Ondersteund |Geen|
| Rich client-toepassingen zoals Lync, Office-abonnement, CRM |  Ondersteund |Geen (eerdere versies moeten een upgrade uitvoeren naar 6.11|
| Uitgebreide e-mail clients zoals Outlook en ActiveSync |  Ondersteund |Geen|

Voor instructies voor het configureren van deze PingFederate STS te bieden de ervaring voor één Active Directory-gebruikers het PDF-bestand downloaden [hier.](http://go.microsoft.com/fwlink/?LinkID=266321)

## <a name="pingfederate-72"></a>7.2 PingFederate 
7.2 PingFederate implementeert de algemeen geaccepteerde norm WS WS-Federation-vertrouwensrelatie identiteit voor een een-op- en kenmerk exchange framework.

Dit is de matrix scenario ondersteuning voor deze ervaring voor eenmalige aanmelding:


| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Web-gebaseerde clients zoals Web Access voor Exchange en SharePoint Online | Ondersteund |Geen|
| Rich client-toepassingen zoals Lync, Office-abonnement, CRM |  Ondersteund |Geen|
| Uitgebreide e-mail clients zoals Outlook en ActiveSync |  Ondersteund |Geen|

Zie voor de PingFederate instructies voor het configureren van deze STS om de ervaring voor één Active Directory-gebruikers en [hier.](http://documentation.pingidentity.com/display/PF72/PingFederate+7.2)

## <a name="pingfederate-8x"></a>PingFederate 8.x 
PingFederate 8.x implementeert de algemeen geaccepteerde norm WS WS-Federation-vertrouwensrelatie identiteit voor een een-op- en kenmerk exchange framework.

Dit is de matrix scenario ondersteuning voor deze ervaring voor eenmalige aanmelding:


| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Web-gebaseerde clients zoals Web Access voor Exchange en SharePoint Online | Ondersteund |Geen|
| Rich client-toepassingen zoals Lync, Office-abonnement, CRM |  Ondersteund |Geen|
| Uitgebreide e-mail clients zoals Outlook en ActiveSync |  Ondersteund |Geen|

Zie voor de PingFederate instructies voor het configureren van deze STS om de ervaring voor één Active Directory-gebruikers en [hier.](http://documentation.pingidentity.com/display/PFS/SSO+to+Office+365+Introduction)

## <a name="centrify"></a>Centrify 
Centrify zorgt voor een federatieve single sign-on ervaring voor Office 365 zonder de noodzaak van een federatieserver in de lokalen die als host fungeert.

Dit is de matrix scenario ondersteuning voor deze ervaring voor eenmalige aanmelding:


| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Web-gebaseerde clients zoals Web Access voor Exchange en SharePoint Online | Ondersteund |Geen|
| Rich client-toepassingen zoals Lync, Office-abonnement, CRM |  Ondersteund |Geen|
| Uitgebreide e-mail clients zoals Outlook en ActiveSync |  Ondersteund |Client Access Control wordt niet ondersteund. 

Zie voor meer informatie over Centrify [hier.](http://www.centrify.com/cloud/apps/single-sign-on-for-office-365.asp)|

## <a name="ibm-tivoli-federated-identity-manager-622"></a>IBM Tivoli federatieve id-beheer 6.2.2 
IBM Tivoli federatieve id-beheer 6.2.2 met IBM Security Access Manager voor Microsoft-toepassingen 1.4 implementeert de algemeen geaccepteerde norm WS WS-Federation-vertrouwensrelatie identiteit voor een een-op- en kenmerk exchange framework.

Dit is de matrix scenario ondersteuning voor deze ervaring voor eenmalige aanmelding: 

| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Web-gebaseerde clients zoals Web Access voor Exchange en SharePoint Online | Ondersteund |Geen|
| Rich client-toepassingen zoals Lync, Office-abonnement, CRM |  Ondersteund |Geen|
| Uitgebreide e-mail clients zoals Outlook en ActiveSync |  Ondersteund |Geen|

Zie voor meer informatie over IBM Tivoli federatieve id-beheer [IBM Security Access Manager voor Microsoft Applications.](http://www-01.ibm.com/support/docview.wss?uid=swg24029517)

## <a name="secureauth-idp-720"></a>IdP SecureAuth 7.2.0 
SecureAuth IdP 7.2.0 implementeert de algemeen geaccepteerde norm WS WS-Federation-vertrouwensrelatie identiteit om een single sign-on ervaring en kenmerk exchange framework.

Dit is de matrix scenario ondersteuning voor deze ervaring voor eenmalige aanmelding: 

| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Web-gebaseerde clients zoals Web Access voor Exchange en SharePoint Online | Ondersteund |Geen|
| Rich client-toepassingen zoals Lync, Office-abonnement, CRM |  Ondersteund |Geen|
| Uitgebreide e-mail clients zoals Outlook en ActiveSync |  Ondersteund |Geen|

Zie voor meer informatie over SecureAuth, [SecureAuth IdP](http://go.microsoft.com/?linkid=9845293).

## <a name="ca-siteminder-1252-sp1-cumulative-release-4"></a>CA SiteMinder 12.52 SP1 cumulatieve Release 4
CA SiteMinder Federatie 12.52 implementeert de algemeen geaccepteerde norm WS WS-Federation-vertrouwensrelatie identiteit voor een een-op- en kenmerk exchange framework.

Dit is de matrix scenario ondersteuning voor deze ervaring voor eenmalige aanmelding: 

| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Web-gebaseerde clients zoals Web Access voor Exchange en SharePoint Online | Ondersteund |Geen|
| Rich client-toepassingen zoals Lync, Office-abonnement, CRM |  Ondersteund |Geen|
| Uitgebreide e-mail clients zoals Outlook en ActiveSync |  Ondersteund |Geen|

Zie voor meer informatie over CA SiteMinder [SiteMinder Federatie van CA.](http://www.ca.com/us/products/ca-single-sign-on.html) 

## <a name="radiantone-cfs-30"></a>RadiantOne 3.0 CFS 
RadiantOne wolk Federation Service (CFS) 3.0 implementeert de algemeen geaccepteerde norm WS WS-Federation-vertrouwensrelatie identiteit voor een een-op- en kenmerk exchange framework.

Dit is de matrix scenario ondersteuning voor deze ervaring voor eenmalige aanmelding: 

| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Web-gebaseerde clients zoals Web Access voor Exchange en SharePoint Online | Ondersteund |Geen|
| Rich client-toepassingen zoals Lync, Office-abonnement, CRM |  Ondersteund |Geïntegreerde Windows-verificatie|
| Uitgebreide e-mail clients zoals Outlook en ActiveSync |  Ondersteund |Geen|

Zie voor meer informatie over het CFS RadiantOne [RadiantOne CFS.](http://www.radiantlogic.com/products/radiantone-cfs/)


## <a name="okta"></a>Okta 
Okta implementeert de algemeen geaccepteerde norm WS WS-Federation-vertrouwensrelatie identiteit voor een een-op- en kenmerk exchange framework.

Dit is de matrix scenario ondersteuning voor deze ervaring voor eenmalige aanmelding: 


| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Web-gebaseerde clients zoals Web Access voor Exchange en SharePoint Online | Ondersteund |Geïntegreerde Windows-verificatie is vereist voor installatie van extra webserver en Okta-toepassing.|
| Rich client-toepassingen zoals Lync, Office-abonnement, CRM |  Ondersteund |Geïntegreerde Windows-verificatie|
| Uitgebreide e-mail clients zoals Outlook en ActiveSync |  Ondersteund |Geen|

Zie voor meer informatie over Okta [Okta.](https://www.okta.com/)
 
## <a name="onelogin"></a>OneLogin 
OneLogin, zoals in mei 2014 getest implementeert de algemeen geaccepteerde norm WS WS-Federation-vertrouwensrelatie identiteit voor een een-op- en kenmerk exchange framework.

Dit is de matrix scenario ondersteuning voor deze ervaring voor eenmalige aanmelding: 

| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Web-gebaseerde clients zoals Web Access voor Exchange en SharePoint Online | Ondersteund |Geïntegreerde Windows-verificatie|
| Rich client-toepassingen zoals Lync, Office-abonnement, CRM |  Ondersteund |Geïntegreerde Windows-verificatie|
| Uitgebreide e-mail clients zoals Outlook en ActiveSync |  Ondersteund |Geen|

Zie voor meer informatie over OneLogin, [OneLogin.](https://www.onelogin.com/)

## <a name="netiq-access-manager-401"></a>NetIQ Access Manager 4.0.1 
NetIQ Access Manager 4.0.1 implementeert de algemeen geaccepteerde norm WS WS-Federation-vertrouwensrelatie identiteit voor een een-op- en kenmerk exchange framework.

Dit is de matrix scenario ondersteuning voor deze ervaring voor eenmalige aanmelding:

| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Web-gebaseerde clients zoals Web Access voor Exchange en SharePoint Online | Ondersteund |* Kerberos contracten worden ondersteund|
| Rich client-toepassingen zoals Lync, Office-abonnement, CRM |  Ondersteund |Geïntegreerde Windows-verificatie wordt niet ondersteund.|
| Uitgebreide e-mail clients zoals Outlook en ActiveSync |  Ondersteund |Geen|

* NetIQ ondersteuning voor Kerberos-verificatie via configuratie van een Contract met Kerberos.  Neem contact op met NetIQ of de setup guide bekijken voor hulp met deze configuratie. Zie voor meer informatie over NetIQ Access Manager [NetIQ Access Manager.](https://www.netiq.com/documentation/netiqaccessmanager4/identityserverhelp/data/b12iqp0m.html)

## <a name="big-ip-with-access-policy-manager-big-ip-ver-113x--116x"></a>BIG-IP met Access Policy Manager BIG-IP ver. 11.3 x – x 11.6 
BIG-IP met Access Policy Manager (APM) ver. BIG-IP 11,3 x – x 11.6 implementeert de algemeen geaccepteerde norm van SAML identiteit om een single sign-on ervaring en kenmerk exchange framework.

Dit is de matrix scenario ondersteuning voor deze ervaring voor eenmalige aanmelding: 

| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Web-gebaseerde clients zoals Web Access voor Exchange en SharePoint Online | Ondersteund |Geen|
| Rich client-toepassingen zoals Lync, Office-abonnement, CRM |  Niet ondersteund |Niet ondersteund|
| Uitgebreide e-mail clients zoals Outlook en ActiveSync |  Ondersteund |Geen|

Zie voor meer informatie over BIG-IP Access Policy Manager [BIG-IP Access Policy Manager.](https://f5.com/products/modules/access-policy-manager) 

Voor instructies voor het configureren van deze BIG-IP Access Policy Manager STS te bieden de ervaring voor één van uw Active Directory-gebruikers het PDF-bestand downloaden [hier.](http://www.f5.com/pdf/deployment-guides/microsoft-office-365-idp-dg.pdf)

## <a name="vmware--workspace-portal-version-21"></a>Portal van de werkruimte VMware versie 2.1 
Portal van de werkruimte VMware versie 2.1 implementeert de algemeen geaccepteerde norm WS WS-Federation-vertrouwensrelatie identiteit voor een een-op- en kenmerk exchange framework.

Dit is de matrix scenario ondersteuning voor deze ervaring voor eenmalige aanmelding:

| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Web-gebaseerde clients zoals Web Access voor Exchange en SharePoint Online | Ondersteund |Geïntegreerde Windows-verificatie wordt niet ondersteund.|
| Rich client-toepassingen zoals Lync, Office-abonnement, CRM | Ondersteund |Geïntegreerde Windows-verificatie wordt niet ondersteund.|
| Uitgebreide e-mail clients zoals Outlook en ActiveSync |  Ondersteund |Geen|

Download de pdf voor meer informatie over Portal van de werkruimte VMware versie 2.1 [hier.](http://pubs.vmware.com/workspace-portal-21/topic/com.vmware.ICbase/PDF/workspace-portal-21-resource.pdf)

## <a name="signgo-53"></a>Log & Start 5.3 
Log & Start 5.3 implementeert de identiteit van de gebruikte WS WS-Federation-vertrouwensrelatie standaard een-op-en kenmerk exchange framework.

Dit is de matrix scenario ondersteuning voor deze ervaring voor eenmalige aanmelding:

| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Web-gebaseerde clients zoals Web Access voor Exchange en SharePoint Online | Ondersteund |Kerberos-contracten wordt ondersteund |
| Rich client-toepassingen zoals Lync, Office-abonnement, CRM | Ondersteund |Geen|
| Uitgebreide e-mail clients zoals Outlook en ActiveSync |  Ondersteund |Geen|


Teken & go 5.3 biedt ondersteuning voor Kerberos-verificatie via configuratie van een Contract met Kerberos.  Hulp bij deze configuratie, neem contact op met Ilex of weergeven van de setup guide [hier.](http://www.ilex-international.com/docs/sign&go_wsfederation_en.pdf)


## <a name="icewall-federation-version-30"></a>IceWall Federatie versie 3.0 
IceWall Federatie versie 3.0 implementeert de algemeen geaccepteerde norm WS WS-Federation-vertrouwensrelatie identiteit voor een een-op- en kenmerk exchange framework.

Dit is de matrix scenario ondersteuning voor deze ervaring voor eenmalige aanmelding:

| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Web-gebaseerde clients zoals Web Access voor Exchange en SharePoint Online | Ondersteund |Geïntegreerde Windows-verificatie wordt niet ondersteund.|
| Rich client-toepassingen zoals Lync, Office-abonnement, CRM | Ondersteund |Geïntegreerde Windows-verificatie wordt niet ondersteund.|
| Uitgebreide e-mail clients zoals Outlook en ActiveSync |  Ondersteund |Geen|

Zie voor meer informatie over de Federatie IceWall [hier](http://h50146.www5.hp.com/products/software/security/icewall/eng/federation/) en [hier.](http://h50146.www5.hp.com/products/software/security/icewall/federation/office365.html)

## <a name="ca-secure-cloud"></a>CA Secure Cloud 

CA Secure Cloud implementeert de algemeen geaccepteerde norm WS WS-Federation-vertrouwensrelatie identiteit voor een een-op- en kenmerk exchange framework.

Dit is de matrix scenario ondersteuning voor deze ervaring voor eenmalige aanmelding:

| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Web-gebaseerde clients zoals Web Access voor Exchange en SharePoint Online | Ondersteund |Geïntegreerde Windows-verificatie wordt niet ondersteund.|
| Rich client-toepassingen zoals Lync, Office-abonnement, CRM | Ondersteund |Geïntegreerde Windows-verificatie wordt niet ondersteund.|
| Uitgebreide e-mail clients zoals Outlook en ActiveSync |  Ondersteund |Geen|

Zie voor meer informatie over CA Secure Cloud [CA Secure Cloud.](http://www.ca.com/us/products/security-as-a-service.aspx)

## <a name="dell-one-identity-cloud-access-manager-v71"></a>Dell een identiteit wolk Access Manager v7.1 
Een identiteit wolk Access Manager van Dell implementeert het algemeen geaccepteerde norm WS WS-Federation-vertrouwensrelatie identiteit voor een een-op- en kenmerk exchange framework.

Dit is de matrix scenario ondersteuning voor deze ervaring voor eenmalige aanmelding:

| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Web-gebaseerde clients zoals Web Access voor Exchange en SharePoint Online | Ondersteund |Geen|
| Rich client-toepassingen zoals Lync, Office-abonnement, CRM |  Ondersteund |Geen|
| Uitgebreide e-mail clients zoals Outlook en ActiveSync |  Ondersteund |Geen|

Zie voor meer informatie over Dell een wolk Access Identiteitenbeheer [Dell een wolk Access Identiteitenbeheer.](http://software.dell.com/products/cloud-access-manager)

 Zie voor instructies voor het configureren van deze STS voor uw Office 365-gebruikers om de ervaring voor één [Office 365-gebruikers configureren.](http://documents.software.dell.com/dell-one-identity-cloud-access-manager/7.1/how-to-configure-microsoft-office-365) 

## <a name="authanvil-single-sign-on-45"></a>AuthAnvil één teken op 4.5 
AuthAnvil één teken op 4.5 implementeert de algemeen geaccepteerde norm WS WS-Federation-vertrouwensrelatie identiteit voor een een-op- en kenmerk exchange framework.

Dit is de matrix scenario ondersteuning voor deze ervaring voor eenmalige aanmelding:

| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Web-gebaseerde clients zoals Web Access voor Exchange en SharePoint Online | Ondersteund |Geïntegreerde Windows-verificatie wordt niet ondersteund.|
| Rich client-toepassingen zoals Lync, Office-abonnement, CRM | Ondersteund |Geïntegreerde Windows-verificatie wordt niet ondersteund.|
| Uitgebreide e-mail clients zoals Outlook en ActiveSync |  Ondersteund |Geen|


Zie voor meer informatie [AuthAnvil Single Sign On.](https://help.scorpionsoft.com/entries/26538603-How-can-I-Configure-Single-Sign-On-for-Office-365-)
