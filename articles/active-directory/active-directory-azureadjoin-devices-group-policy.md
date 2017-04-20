<properties
    pageTitle="Domein behoren apparaten aansluiten op Azure AD voor Windows 10 ervaringen | Microsoft Azure"
    description="Wordt beschreven hoe beheerders kunnen Groepsbeleid configureren om te apparaten zijn domein zijn verbonden met het bedrijfsnetwerk."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="connect-domain-joined-devices-to-azure-ad-for-windows-10-experiences"></a>Domein behoren apparaten aansluiten op Azure AD voor Windows 10 ervaringen

Aan domein toevoegen is dat de traditionele manier organisaties aangesloten apparaten voor werk voor de laatste 15 jaar, en nog veel meer. Gebruikers kunnen zich aanmelden bij hun apparaten met behulp van hun Windows Server Active Directory (Active Directory) werk of school accounts ingeschakeld en toegestaan IT volledig deze apparaten te beheren. Organisaties zijn meestal afhankelijk van imaging methoden voor bepaling van apparaten voor gebruikers en in het algemeen gebruik van System Center Configuration Manager (SCCM) of Groepsbeleid te beheren.

Aan domein toevoegen in Windows 10 biedt de volgende voordelen nadat u apparaten op Azure Active Directory (AD Azure aansluit):

- Eenmalige aanmelding (SSO) met Azure AD bronnen vanaf elke locatie
- Toegang tot de onderneming Windows Store via werk of school accounts (geen Microsoft-account vereist)
- Enterprise-compatibele zwervende gebruiker instellingen tussen apparaten via werk of school accounts (geen Microsoft-account vereist)
- Sterke verificatie en gemakkelijk aanmelden voor werk of school met Microsoft Passport en Windows Hello
- Mogelijkheid om de toegang beperken tot alleen apparaten die voldoen aan de organisatie-instellingen voor Groepsbeleid

## <a name="prerequisites"></a>Vereisten

Aan domein toevoegen, nog steeds nuttig. Echter op de Azure AD voordelen van SSO, zwerven van met werk of school-accounts en -instellingen naar de Windows Store met werk of school-accounts, moet u het volgende:

- Azure AD-abonnement
- Azure AD verbinding maken met de map op het bedrijf uitbreiden naar Azure AD
- Beleid dat is ingesteld op domein behoren apparaten aansluiten op Azure AD
- Windows 10 build (build 10551 of nieuwer) voor apparaten

Als u Microsoft Passport voor werk- en Windows Hello, moet u ook het volgende:

- Openbare-sleutelinfrastructuur (PKI) voor de gebruiker certificaten afgeven.
- System Center Configuration Manager versie 1509 voor technische Preview. Zie [Microsoft System Center Configuration Manager technische Preview](https://technet.microsoft.com/library/dn965439.aspx#BKMK_TP3Update) en [System Center Configuration Manager Team Blog](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx)voor meer informatie. Dit is vereist voor de implementatie van gebruikerscertificaten op basis van een Microsoft Passport-sleutels.

Als alternatief voor de vereiste PKI-implementatie kunt u het volgende doen:

- Hebben een aantal domeincontrollers met Windows Server 2016 Active Directory Domain Services.

Voor voorwaardelijke toegang, kunt u instellingen voor Groepsbeleid hebt u toegang tot een domein behoren apparaten met geen aanvullende installaties. Voor het beheren van toegangsbeheer op basis van de overeenstemming van het apparaat, moet u het volgende:

- System Center Configuration Manager versie 1509 voor technische Preview voor Passport-scenario 's

## <a name="deployment-instructions"></a>Aanwijzingen voor implementatie



### <a name="step-1-deploy-azure-active-directory-connect"></a>Stap 1: Implementatie van Azure Active Directory verbinden

Azure AD verbinden, kunt u computers op gebouwen inrichten als apparaatobjecten in de cloud. Verwijzen naar "Verbinden Azure AD installeren" in het artikel in [uw identiteiten op ruimten met Azure Active Directory integreren](active-directory-aadconnect.md#install-azure-ad-connect)voor de implementatie van Azure AD verbinden.

 - Als u een [aangepaste installatie voor het verbinden van Azure AD](./connect/active-directory-aadconnect-get-started-custom.md) (niet de Express-installatie) gevolgd, volgt u de procedure **een serviceverbindingspunt in Active Directory op ruimten maken**, verderop in deze stap.
 - Als u voert een federatieve configuratie met Azure Active Directory voordat u Azure AD verbinden (bijvoorbeeld, als u Active Directory Federation Services (AD FS) voordat hebt geïnstalleerd), installeert u vervolgens de procedure **regels claim voor AD FS configureren** verderop in deze stap.

#### <a name="create-a-service-connection-point-in-on-premises-active-directory"></a>Een serviceverbindingspunt in Active Directory voor gebouwen maken

Domein behoren apparaten gebruiken om informatie aan de huurder Azure AD ten tijde van de automatische registratie met de registratieservice Azure apparaat het serviceverbindingspunt.

Voer de volgende PowerShell-opdrachten op de server verbinden met Azure AD:

    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;


Bij het uitvoeren van de cmdlet $aadAdminCred = Get-Credential, gebruikt u de notatie *user@example.com* voor de gebruikersnaam van de referentie die wordt ingevoerd wanneer u de Get-Credential pop-upvenster weergegeven.

Bij het uitvoeren van de cmdlet initialiseren ADSyncDomainJoinedComputerSync... vervangen door [*naam connector*] de domeinaccount dat wordt gebruikt als de Active Directory connector-account.

#### <a name="configure-ad-fs-claim-rules"></a>AD FS-claims regels configureren
De AD FS-claims regels configureren, kunnen onmiddellijke registratie van een computer met Azure apparaat registratieservice doordat computers worden geverifieerd via Kerberos/NTLM via AD FS. Zonder deze stap krijgt computers naar Azure AD zodanig vertraagd (afhankelijk van de Azure AD maken sync tijd).

>[AZURE.NOTE]
Als er geen AD FS als de federation-server in-ruimten, volg de instructies van de leverancier om de regels van de claim te maken.

Voer de volgende PowerShell-opdrachten op de AD FS-server (of op een sessie is verbonden met de AD FS-server):

      <#----------------------------------------------------------------------
     |   Modify the Azure AD Relying Party to include the claims needed
     |   for DomainJoin++. The rules include:
     |   -ObjectGuid
     |   -AccountType
     |   -ObjectSid
     +---------------------------------------------------------------------#>

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules

    $rule1 = '@RuleName = "Issue object GUID"
          c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&
          c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(store = "Active Directory", types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), query = ";objectguid;{0}", param = c2.Value);'

    $rule2 = '@RuleName = "Issue account type for domain joined computers"
          c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "DJ");'

    $rule3 = '@RuleName = "Pass through primary SID"
          c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&
          c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(claim = c2);'

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString

>[AZURE.NOTE]
Windows 10 computers wordt met behulp van geïntegreerde Windows-verificatie voor een actieve WS-Trust eindpunt gehost door AD FS geverifieerd. Zorg ervoor dat dit eindpunt is ingeschakeld. Als u de webproxy voor verificatie gebruikt, ook voor zorgen dat dit eindpunt is gepubliceerd via de proxy. U kunt dit doen door de AD FS/services/vertrouwen/13/windowstransport controleren. Moet worden weergegeven in de beheerconsole van AD FS onder **Service**ingeschakeld > **eindpunten**.


### <a name="step-2-configure-automatic-device-registration-via-group-policy-in-active-directory"></a>Stap 2: Automatische registratie via Groepsbeleid in Active Directory configureren

U kunt Groepsbeleid in Active Directory Windows 10 domein behoren apparaten automatisch registreren met Azure Active Directory configureren.

> [AZURE.NOTE]
> Nieuwste Zie voor instructies voor het instellen van automatische registratie, [het instellen van automatische inschrijving van een Windows-domein verbonden apparaten met Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).
>
> Deze groepsbeleidsjabloon heeft in Windows 10 gekregen. Als u het hulpprogramma Groepsbeleid vanaf een computer met Windows 10 uitvoert, wordt het beleid wordt weergegeven als: <br>
> **Computers die lid van domein registreren als apparaten**<br>
> Het beleid is op de volgende locatie:<br>
> ***Computer configuratie/beleid/Administrative Templates/Windows Components/registratie***


## <a name="additional-information"></a>Als u meer informatie
* [Windows 10 voor de onderneming: apparaten gebruik voor werk](active-directory-azureadjoin-windows10-devices-overview.md)
* [Cloud-mogelijkheden aan Windows 10-apparaten via Azure Active Directory deelnemen aan uitbreiden](active-directory-azureadjoin-user-upgrade.md)
* [Meer informatie over gebruiksscenario's voor Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Domein behoren apparaten aansluiten op Azure AD voor Windows 10 ervaringen](active-directory-azureadjoin-devices-group-policy.md)
* [Azure AD Join instellen](active-directory-azureadjoin-setup.md)
