<properties
    pageTitle="Azure AD verbinden: Poorten | Microsoft Azure"
    description="Deze pagina is een pagina met technische naslaginformatie voor poorten die nodig zijn om te worden gebruikt voor het verbinden van Azure AD"
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
    ms.date="08/25/2016"
    ms.author="billmath"/>

# <a name="hybrid-identity-required-ports-and-protocols"></a>Hybride identiteit vereiste poorten en protocollen
Het volgende document bevat technische naslaginformatie informatie te verstrekken over de vereiste poorten en protocollen die vereist zijn voor de uitvoering van een identiteit hybride oplossing. Gebruik de volgende afbeelding en verwijzen naar de overeenkomstige tabel.

![Wat is Azure AD verbinden](./media/active-directory-aadconnect-ports/required1.png)

## <a name="table-1---azure-ad-connect-and-on-premises-ad"></a>Tabel 1 - Azure AD verbinding en On-premises AD
Deze tabel beschrijft de poorten en protocollen die vereist voor de communicatie tussen de server verbinden met Azure AD zijn en AD ruimten.

Protocol | Poorten | Beschrijving
--------- | --------- |---------
DNS|53 (TCP/UDP)| DNS-lookups op het doelforest.
Kerberos|88 (TCP/UDP)| Kerberos-verificatie aan de AD-forest.
MS-RPC |135 (TCP/UDP)| Gebruikt tijdens de eerste configuratie van de wizard Azure AD verbinden wanneer deze gekoppeld aan de AD-forest.
LDAP|389 (TCP/UDP)| Gebruikt voor het importeren van gegevens uit Active Directory. Gegevens worden gecodeerd met Kerberos-teken en zegel.
LDAP/SSL|636 (TCP/UDP)| Gebruikt voor het importeren van gegevens uit Active Directory. De overdracht van gegevens is ondertekend en gecodeerd. Alleen als u SSL gebruikt.
RPC |49152 - 65535 (willekeurig hoge RPC Port)(TCP/UDP)| Gebruikt tijdens de eerste configuratie van Azure AD verbinden wanneer deze gekoppeld aan de AD-forests. [KB929851](https://support.microsoft.com/kb/929851), [KB832017](https://support.microsoft.com/kb/832017)en [KB224196](https://support.microsoft.com/kb/224196) Zie voor meer informatie.

## <a name="table-2---azure-ad-connect-and-azure-ad"></a>Tabel 2 - Azure AD verbinding en Azure AD
Deze tabel wordt beschreven voor de poorten en protocollen die nodig voor de communicatie tussen de server verbinden met Azure AD en Azure AD zijn.

Protocol |Poorten |Beschrijving
--------- | --------- |---------
HTTP|80 (TCP/UDP)| Gebruikt voor het downloaden van CRL's (certificaatintrekkingslijsten) om te controleren of de SSL-certificaten.
HTTPS|443(TCP/UDP)| Synchroniseren met AD Azure gebruikt.

Voor een lijst met URL's en IP-adressen die u wilt openen in de firewall, Zie [Office 365-URL's en IP-adresbereiken](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

## <a name="table-3---azure-ad-connect-and-federation-serverswap"></a>Tabel 3 - Azure AD verbinding en Federation-Servers WAP
In deze tabel worden de poorten en protocollen die nodig voor de communicatie tussen de server verbinden met Azure AD en WAP Federation-servers zijn beschreven.  

Protocol |Poorten |Beschrijving
--------- | --------- |---------
HTTP|80 (TCP/UDP)| Gebruikt voor het downloaden van CRL's (certificaatintrekkingslijsten) om te controleren of de SSL-certificaten.
HTTPS|443(TCP/UDP)| Synchroniseren met AD Azure gebruikt.
WinRM|5985| WinRM-Listener

## <a name="table-4---wap-and-federation-servers"></a>Tabel 4 - WAP en Federation-Servers
Deze tabel wordt beschreven voor de poorten en protocollen die nodig voor de communicatie tussen de Federation-servers en servers van de WAP zijn.

Protocol |Poorten |Beschrijving
--------- | --------- |---------
HTTPS|443(TCP/UDP)| Gebruikt voor verificatie.

## <a name="table-5---wap-and-users"></a>Tabel 5 - WAP en gebruikers
In deze tabel worden de poorten en protocollen die nodig voor de communicatie tussen gebruikers en de WAP-servers zijn beschreven.

Protocol |Poorten |Beschrijving
--------- | --------- |--------- |
HTTPS|443(TCP/UDP)| Gebruikt voor verificatie van apparaat.
TCP|49443 (TCP)| Gebruikt voor verificatie.

## <a name="table-6a--6b---azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>Tabel 6a en 6b - Azure AD verbinding Health agent voor (AD FS/synchronisatie) en Azure AD
De volgende tabellen beschrijven de eindpunten, poorten en protocollen die vereist voor de communicatie tussen Azure AD verbinding Health agents en Azure AD zijn

### <a name="table-6a---ports-and-protocols-for-azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>Tabel 6a - poorten en protocollen voor Azure AD verbinding Health agent voor (AD FS/synchronisatie) en Azure AD
Deze tabel wordt beschreven voor de volgende uitgaande poorten en protocollen die nodig voor de communicatie tussen de Azure AD verbinding Health agents en Azure AD zijn.  

Protocol |Poorten  |Beschrijving
--------- | --------- |--------- |
HTTPS|443(TCP/UDP)| Uitgaande
Azure Service Bus|5671 (TCP/UDP)| Uitgaande

### <a name="6b---endpoints-for-azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>6B - eindpunten voor Azure AD verbinding Health agent voor (AD FS/synchronisatie) en Azure AD
Zie [de sectie vereisten voor de Azure AD verbinding Health agent](active-directory-aadconnect-health-agent-install.md#requirements)voor een lijst van eindpunten.
