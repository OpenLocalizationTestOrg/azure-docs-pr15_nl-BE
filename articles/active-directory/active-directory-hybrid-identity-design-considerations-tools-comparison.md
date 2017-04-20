<properties
    pageTitle="Hybride identiteit: Directory integration tools vergelijking | Microsoft Azure"
    description="Deze pagina bevat een uitgebreide tabel vergelijkt de verschillende directory integratie hulpmiddelen kunnen worden gebruikt voor directory-integratie is."
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
    ms.topic="get-started-article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="hybrid-identity-directory-integration-tools-comparison"></a>Hybride identiteit directory integration tools vergelijking

De integratie van directory's hebben in de jaren gegroeid en geëvolueerd.  Dit document is te voorzien van een gecombineerde weergave van deze hulpprogramma's en een vergelijking van de functies die beschikbaar in elk zijn.

<!-- The hardcoded link is a workaround for campaign ids not working in acom links-->

>[AZURE.NOTE] Onderdelen en functionaliteit die eerder zijn uitgebracht als Dirsync en AAD Sync is uitgerust met Azure AD verbinden. Deze hulpprogramma's zijn niet langer afzonderlijk gepubliceerd en alle toekomstige verbeteringen worden opgenomen in updates voor Azure AD Connect, zodat u altijd weet waar u de meest recente functies.
>
>DirSync en Azure AD Sync zijn afgeschaft. Meer informatie vindt u [hier](active-directory-aadconnect-dirsync-deprecated.md)in.


Gebruik de volgende sleutel voor elk van de tabellen.

● = Nu beschikbaar  
FR = toekomstige Release  
PP = Public Preview  

## <a name="on-premises-to-cloud-synchronization"></a>On-Premises cloud synchronisatie

| Functie  | Azure Active Directory-verbinding  | Azure Active Directory Synchronization Services (AAD synchronisatie) | Azure Active Directory Synchronization Tool (DirSync)| Forefront Identiteitenbeheer 2010 R2 (FIM) |Microsoft Identity Manager 2016 (MIM)|
| :-------- |:--------:|:--------:|:--------:|:--------:|:--------:
| Verbinding maken met één forest AD ruimten | ● | ● | ● | ● |● |
| Verbinding maken met meerdere forests voor AD ruimten |●  | ● |  | ● |● |
| Verbinding maken met meerdere op ruimten Exchange Orgs | ● |  |  |  | |
| Verbinding maken met de LDAP-adreslijst op ruimten is één | FR |  |  | ● |● |
| Verbinding maken met meerdere LDAP-adreslijsten in gebouwen |FR  |  |  | ● |● |
| Verbinding maken met AD ruimten en LDAP-adreslijsten ruimten |FR  |  |  | ● |● |
| Verbinding maken met aangepaste systemen (dat wil zeggen SQL, Oracle, MySQL, enz.) | FR |  |  | ● |● |
| Synchroniseer de klant gedefinieerde kenmerken (extensions directory) | ● |  |  |  |  |
|Verbinding maken met HR voor bedrijfsruimten (dat wil zeggen, SAP, Oracle, eBusiness, PeopleSoft)| FR |  |  | ● |● |
|FIM synchronisatieregels en aansluitingen ondersteunt voor het inrichten van systemen voor de lokalen.|  |  |  | ● |● |

## <a name="cloud-to-on-premises-synchronization"></a>Wolk met On-Premises synchronisatie

| Functie  | Azure Active Directory-verbinding  | Azure Active Directory Synchronization Services | Azure Active Directory Synchronization Tool (DirSync) | Forefront Identiteitenbeheer 2010 R2 (FIM) |Microsoft Identity Manager 2016 (MIM)|
| :-------- |:--------:|:--------:|:--------:|:--------:|:--------:
| Write-back van apparaten | ● |  | ● |  ||
| Kenmerk Write-back (voor de implementatie van Exchange-hybride) | ● | ● | ● | ● |● |
| Write-back van gebruikers en groepen objecten |  ●|  | |  ||
| Write-back van wachtwoorden (van self-wachtwoord opnieuw instellen (SSPR) en het wachtwoord wijzigen) |  ● | ● |  |  ||



## <a name="authentication-feature-support"></a>Functie voor ondersteuning van verificatie

| Functie  | Azure Active Directory-verbinding | Azure Active Directory Synchronization Services | Azure Active Directory Synchronization Tool (DirSync) | Forefront Identiteitenbeheer 2010 R2 (FIM) |Microsoft Identity Manager 2016 (MIM)|
| :-------- |:--------:|:--------:|:--------:|:--------:|:--------:
| Password-Sync voor één op-premises AD forest | ● | ● | ● |  ||
| Password-Sync voor meerdere in-premises AD forests |  ●| ● |  |  ||
| Single Sign-on met Federation | ● | ● | ● | ● |● |
| Write-back van wachtwoorden (van SSPR en het wachtwoord wijzigen) |●  | ● |  |  ||



## <a name="set-up-and-installation"></a>Installatie

| Functie  | Azure Active Directory-verbinding  | Azure Active Directory Synchronization Services | Azure Active Directory Synchronization Tool (DirSync) | Microsoft Identity Manager 2016 (MIM) |
| :-------- |:--------:|:--------:|:--------:|:--------:
| Biedt ondersteuning voor installatie op een domeincontroller | ● | ● | ● |  |
| Met behulp van SQL Express-installatie ondersteunt | ● | ● | ● |  |
| Eenvoudige upgrade van DirSync |● |  |  |  |
| Lokalisatie van Admin UX op Windows Server-talen | ● | ● | ● |  |
|Lokalisatie van de eindgebruiker UX talen in Windows Server| |  |  |● |
| Ondersteuning voor Windows Server 2008 en Windows Server 2008 R2 | ● voor synchronisatie, niet voor een Federatie| ● | ●  | ● |
| Ondersteuning voor Windows Server 2012 en Windows Server 2012 R2 | ● | ● | ● | ● |

## <a name="filtering-and-configuration"></a>Filteren en configuratie

Functie  | Azure Active Directory-verbinding | Azure Active Directory Synchronization Services | Azure Active Directory Synchronization Tool (DirSync) | Forefront Identiteitenbeheer 2010 R2 (FIM)| Microsoft Identity Manager 2016 (MIM)
:-------- |:--------:|:--------:|:--------:|:--------:|:--------:|
Filteren op domeinen en organisatie-eenheden | ● | ● | ● | ●  | ●
Filteren op waarden van kenmerk van het object | ● | ● | ● | ●| ●
Toestaan de minimale set kenmerken moeten worden gesynchroniseerd (MinSync) | ● | ● |  ||
Toestaan dat verschillende sjablonen moeten worden toegepast voor overdrachten van kenmerk |●  | ● |  ||
Kenmerken van die voortvloeien uit AD naar Azure AD verwijderen toestaan | ● | ● |  |  |
Geavanceerde aanpassingen voor attribuut overdrachten toestaan | ● | ● |  | ●  | ●

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de [integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md).
