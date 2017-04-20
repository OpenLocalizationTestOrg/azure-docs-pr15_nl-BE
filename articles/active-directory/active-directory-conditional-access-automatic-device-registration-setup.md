<properties
    pageTitle="Automatische registratie van Windows-domein behoren apparaten met Azure Active Directory instellen | Microsoft Azure"
    description="Uw Windows-domein behoren apparaten instellen automatisch en ongemerkt met Azure Active Directory registreren."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="markvi"/>



# <a name="set-up-automatic-registration-of-windows-domain-joined-devices-with-azure-active-directory"></a>Automatische registratie van Windows instellen met apparaten met Azure Active Directory domein behoren

Voor het gebruik van [voorwaardelijke toegang Azure Active Directory op basis van het apparaat](active-directory-conditional-access.md), moeten uw Windows-domein behoren computers met Azure Active Directory (AD Azure) zijn geregistreerd. In dit artikel wordt beschreven wat u moet doen om de registratie van Windows-domein behoren apparaten met Azure Active Directory in uw organisatie in te stellen.

Met behulp van voorwaardelijke toegang in Azure AD biedt deze voordelen:

-   Verbeterde eenmalige aanmelding (SSO) ervaring in Azure AD apps via werk of school
-   Enterprise zwervende instellingen tussen apparaten
-   Toegang tot de Windows Store voor bedrijven
-   Sterke verificatie en gemakkelijk aanmelden bij Windows Hello

> [AZURE.NOTE] De Update voor Windows 10 November biedt enkele van de uitgebreide ervaringen in Azure AD, maar de Update Windows 10 Verjaardag biedt volledige ondersteuning voor voorwaardelijke toegang op basis van het apparaat. Zie voor meer informatie over voorwaardelijke toegang, [voorwaardelijke toegang Azure Active Directory op basis van het apparaat](active-directory-conditional-access.md). Zie voor meer informatie over Windows 10-apparaten in de werkplek en hoe een gebruiker een Windows 10 apparaat registreert met Azure AD [Windows 10 voor de onderneming: apparaten gebruiken voor](active-directory-azureadjoin-windows10-devices-overview.md).

Sommige eerdere versies van Windows, met inbegrip van deze versies kunt u registreren:

-   Windows 8.1
-   Windows 7

Als u een Windows Server-computer als een desktopcomputer gebruikt, kunt u deze platforms registreren:

- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2


## <a name="prerequisites"></a>Vereisten

De belangrijkste vereiste voor automatische registratie van apparaten met behulp van Azure AD deel uitmaakt van een domein is een bijgewerkte versie van Azure Active Directory (Azure AD verbinding maken verbinding maken).

Afhankelijk van hoe u verbinden met Azure AD geïmplementeerd en wordt aangegeven of u gebruikt een uitdrukkelijke of aangepaste installatie of een upgrade, de volgende vereisten mogelijk is gevonden:

-   **Serviceverbindingspunt in op-premises Active Directory**. Voor de detectie van Azure AD huurder informatie door computers die voor Azure AD registreren.

-   **Active Directory Federation Services (AD FS) uitgifte transformeren regels**. Voor computerverificatie op registratie (geldt voor federatieve configuraties).

Als sommige apparaten in uw organisaties niet deel uitmaakt van een domein Windows 10-apparaten, moet dat u de volgende taken uitvoeren:

- Een beleid instellen in Azure AD, zodat gebruikers kunnen apparaten registreren
- Geïntegreerde Windows-verificatie ingesteld als een geldig alternatief voor meerledige verificatie in AD FS



## <a name="set-a-service-connection-point-for-discovery-of-the-azure-ad-tenant"></a>Een serviceverbindingspunt voor de detectie van de pachter Azure AD instellen

Een service connection point-object moet bestaan in de configuratienaamgevingspartitie context van het forest van het domein waar computers worden gekoppeld. Het service connection point bevat gegevens over de huurder Azure AD discovery waar computers worden geregistreerd. In een forest met meerdere Active Directory-configuratie, moet het serviceverbindingspunt bestaan in alle forests met computers van een domein behoren.

Het service connection point op deze locaties in Active Directory instellen:

    CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,[Configuration Naming Context]

> [AZURE.NOTE] Voor een forest met de Active Directory-domein naam *voorbeeld.com*, de naamgevingscontext Configuratie CN is = Configuration, DC = voorbeeld, DC = com.

U kunt controleren voor de Azure AD huurder object en detectie met behulp van de volgende Windows PowerShell-script. (De configuratienaamgevingscontext in het voorbeeld vervangen door de configuratienaamgevingscontext.)

    $scp = New-Object System.DirectoryServices.DirectoryEntry;

    $scp.Path = "LDAP://CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=example,DC=com";

    $scp.Keywords;

De `$scp.Keywords` uitvoer geeft de huurder Azure AD informatie:

    azureADName:microsoft.com

    azureADId:72f988bf-86f1-41af-91ab-2d7cd011db47

Als het serviceverbindingspunt niet bestaat, maakt u met behulp van de volgende PowerShell script op de server verbinden met Azure AD:

    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;


> [AZURE.NOTE]Bij het uitvoeren van `$aadAdminCred = Get-Credential`, gebruikt u de notatie *user@example.com* voor de gebruikersnaam in het dialoogvenster **Get referentie** .  
> Als u de initialisatie-ADSyncDomainJoinedComputerSync-cmdlet uitvoert, vervangen door [*naam connector*] de domeinaccount dat wordt gebruikt in de Active Directory connector-account.  
> De cmdlet gebruikt de module Active Directory PowerShell, dat op Active Directory Web Services in een domeincontroller steunt. Active Directory Web Services wordt ondersteund op domeincontrollers in Windows Server 2008 R2 en hogere versies. De API System.DirectoryServices via PowerShell gebruiken voor het maken van het serviceverbindingspunt voor domeincontrollers in Windows Server 2008 of eerdere versies, en vervolgens de waarden trefwoorden toewijzen.

## <a name="create-ad-fs-rules-for-instant-device-registration-in-federated-organizations"></a>AD FS regels maken voor onmiddellijke registratie in federatieve organisaties

In een federatieve Azure AD-configuratie apparaten zijn gebaseerd op AD FS (of op de federatieserver in gebouwen) voor de verificatie van AD Azure. Vervolgens registreren ze tegen Azure Active Directory apparaat registratieservice (Service Azure AD apparaat registratie).

> [AZURE.NOTE] In een configuratie met niet-federatieve gebruiker wachtwoord-hashes worden gesynchroniseerd met AD Azure en Windows 10 en Windows Server 2016 domein behoren computers worden geverifieerd bij de registratieservice Azure AD-apparaat. Een gebruiker wordt geverifieerd met behulp van een referentie die de gebruiker in hun computeraccounts op gebouwen schrijft en die is doorgegeven aan de Azure AD via Azure AD verbinden. Voor Windows 10 en Windows Server 2016 computers in een niet-federatieve configuratie hebt u de opties voor het instellen van een CA op basis van het apparaat in uw organisatie. Zie de sectie downloaden van Windows Installer-pakketten voor Windows 10 computers in dit artikel voor meer informatie.

Voor computers met Windows 10 en Windows Server 2016 Azure AD Connect worden gekoppeld aan het apparaatobject in Azure AD het computeraccountobject in gebouwen. De volgende vorderingen moeten aanwezig zijn tijdens de verificatie voor Azure AD apparaat registratieservice inschrijving voltooien en het apparaatobject maken:

- http://schemas.Microsoft.com/ws/2012/01/accounttype de DJ waarde bevat, waarmee de voornaamste verificator als een computer deel uitmaakt van een domein.
- http://schemas.Microsoft.com/Identity/claims/onpremobjectguid bevat de waarde van het kenmerk **objectGUID** van de computeraccount op gebouwen.
- http://schemas.Microsoft.com/ws/2008/06/Identity/claims/primarysid bevat van de computer primaire SID (security identifier), die overeenkomt met de waarde van de **objectSid** attribuut van de computeraccount op ruimten.
- http://schemas.Microsoft.com/ws/2008/06/Identity/claims/issuerid bevat de waarde die Azure AD gebruikt voor het vertrouwen van het token van AD FS of van de op ruimten Security Token Service (STS) uitgegeven. Dit is belangrijk in een forest met meerdere Active Directory-configuratie. In deze configuratie kunnen computers aan een forest dan het Azure AD op de AD FS maakt of STS ruimten worden toegevoegd. Gebruik de waarde voor de AD FS http://<*domeinnaam*> AD FS/services/vertrouwen /, waarbij <*domeinnaam*> de gevalideerde domeinnaam in Azure AD.

Gebruik de volgende PowerShell script in een sessie die is aangesloten op de server om deze regels handmatig te maken in AD FS. De eerste regel met de domeinnaam van uw organisatie gevalideerde in Azure AD vervangen.

> [AZURE.NOTE] Als u geen AD FS voor de federation-server in de lokalen gebruikt, instructies van de leverancier voor het maken van de regels die deze claims worden verleend.

    $validatedDomain = "example.com"      # Replace example.com with your organization's validated domain name in Azure AD

    $rule1 = '@RuleName = "Issue object GUID"

        c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&

        c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]

        => issue(store = "Active Directory", types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), query = ";objectguid;{0}", param = c2.Value);'

    $rule2 = '@RuleName = "Issue account type for domain-joined computers"

      c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]

      => issue(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "DJ");'

> [AZURE.NOTE] Gebruik alleen de eerste drie regels als uw omgeving op locatie is één enkel forest. Als de computers zich in een ander forest dan één synchroniseren met Azure Active Directory of als u alternatieve namen voor de reeksen die in de synchronisatieconfiguratie, moet u ook de resterende regels.

    $rule3 = '@RuleName = "Pass through primary SID"

      c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&

      c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]

      => issue(claim = c2);'

    $rule4 = '@RuleName = "Issue AccountType with the value User when it’s not a computer account"

      NOT EXISTS([Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "DJ"])

      => add(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "User");'

    $rule5 = '@RuleName = "Capture UPN when AccountType is User and issue the IssuerID"

      c1:[Type == "http://schemas.xmlsoap.org/claims/UPN"] &&

      c2:[Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "User"]

      => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c1.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));'

    $rule6 = '@RuleName = "Update issuer for DJ computer auth"

      c1:[Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "DJ"]

      => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = "http://'+$validatedDomain+'/adfs/services/trust/");'

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3 + $rule4 + $rule5 + $rule6

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString

> [AZURE.NOTE] Windows 10 en Windows Server 2016 domein behoren computers verifiëren met behulp van geïntegreerde Windows naar een actieve WS-Trust eindpunt dat wordt gehost door AD FS. Zorg ervoor dat het eindpunt actief is. Als u de toepassing Web Proxy gebruikt, moet u dat dit eindpunt is gepubliceerd via de proxy. Het eindpunt is AD FS/services/vertrouwen/13/windowstransport. Controleren of actief in de AD FS-beheerconsole Ga naar de **Service** > **eindpunten**. Als u geen AD FS voor de federation-server in de lokalen gebruikt, instructies van de leverancier om ervoor te zorgen dat de bijbehorende eindpunt actief is.



## <a name="set-up-ad-fs-for-authentication-of-device-registration"></a>Instellen van AD FS voor de verificatie van de registratie

Zorg ervoor dat uitgebreid is ingesteld als een geldig alternatief voor meerledige verificatie voor de registratie van AD FS. Hiervoor moet u een regel die wordt doorgegeven via de verificatiemethode transformeren uitgifte.

1. Ga in de beheerconsole van AD FS met **AD FS** > **Vertrouwensrelaties** > **Partij vertrouwt vertrouwen**.

2. Klik met de rechtermuisknop op het Microsoft Office 365 identiteit Platform gebruikmakende partij vertrouwensobject en selecteer **Bewerken Claim regels**.

3.  Selecteer de **Regel toevoegen**op het tabblad **Regels voor uitgifte transformeren** .

4.  Selecteer **verzenden van vorderingen met behulp van een aangepaste regel**in de lijst van de sjabloon **Claim regel** .

5.  Selecteer **volgende**.

6.  Voer in het vak **naam van Claim regel** **Auth methode Claim regel**.

7.  Voer de volgende opdracht in het vak **regel Claim** :

    `c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"]
    => issue(claim = c);`.

8. Voer de volgende PowerShell-opdracht op de federatieserver:

    `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

<*RPObjectName*> is de naam van het gebruikmakende partij voor uw object Azure AD gebruikmakende partij vertrouwen. Dit object is meestal de naam *Identiteit Platform van Microsoft Office 365*.



## <a name="deployment-and-rollout"></a>Installatie en implementatie

Wanneer een domein behoren computers voldoen aan de vereisten, gaan ze met Azure Active Directory registreren.

Windows 10 trouwdag Update en Windows Server 2016 domein behoren computers registreren automatisch met Azure AD zodra het apparaat opnieuw wordt gestart of wanneer een gebruiker zich aanmeldt bij Windows. Nieuwe computers die lid zijn van het domein registreren met Azure Active Directory wanneer het apparaat wordt opnieuw na de join-bewerking van het domein opgestart.

> [AZURE.NOTE] Windows 10 domein behoren computers registreren automatisch met Azure Active Directory alleen mogelijk als het groepsbeleidsobject implementatie.

U kunt een groepsbeleidsobject de invoering van de automatische registratie van Windows 10 en Windows Server 2016 domein behoren computers beheren. Om te zetten automatische registratie van Windows 10-computers deel uitmaakt van een domein, kunt u een Windows Installer-pakket implementeren op computers die u hebt geselecteerd.

> [AZURE.NOTE] Het groepsbeleid voor implementatie besturingselement activeert ook de registratie van Windows 8.1 domein behoren computers. U kunt het beleid voor het registreren van Windows 8.1 domein behoren computers. Of hebt u een combinatie van Windows-versies, met inbegrip van de versies van Windows 7 of Windows Server, kunt u alle Windows 10 en computers met Windows Server 2016 registreren met behulp van een Windows Installer-pakket.

### <a name="create-a-group-policy-object-to-control-the-rollout-of-automatic-registration"></a>Maak een groepsbeleidsobject voor de invoering van automatische inschrijving

Voor de invoering van de automatische registratie van computers met Azure Active Directory domein behoren, kunt u het groepsbeleid voor het **registreren van een domein behoren computers als apparaten** implementeren op de computers die u wilt registreren. U kunt bijvoorbeeld het beleid implementeren een beveiligingsgroep of een organisatie-eenheid.

Als u wilt dat het beleid, voer deze stappen uit:

1. Open Serverbeheer en Ga naar **Extra** > **Voor Groepsbeleidsbeheer**.

2. Ga naar het knooppunt dat overeenkomt met het domein waar u automatische registratie van Windows 10 of Windows Server 2016 computers te activeren.

3. Klik met de rechtermuisknop op **Groepsbeleidsobjecten**en selecteer vervolgens **Nieuw**.

4. Voer een naam voor het groepsbeleidsobject. Bijvoorbeeld: *Automatische registratie naar Azure AD*. Klik op **OK**.

4. Klik met de rechtermuisknop op het nieuwe groepsbeleidsobject en selecteer vervolgens **bewerken**.

5. Ga naar **Computerconfiguratie** > **beleid** > **Beheersjablonen** > **Windows-onderdelen** > **registratie**. Met de rechtermuisknop op **Register domein verbonden computers als apparaten**en selecteer vervolgens **bewerken**.

    > [AZURE.NOTE] Deze sjabloon is gewijzigd van eerdere versies van de console Groepsbeleidsbeheer. Als u een eerdere versie van de console gebruikt, gaat u naar **Computerconfiguratie** > **beleid** > **Beheersjablonen** > **Windows-onderdelen** > **Werkplek Join** > **automatisch werkplek join clientcomputers**.

6. Selecteer **ingeschakeld**en selecteer **toepassen**.

7. Klik op **OK**.

8. Het groepsbeleidsobject koppelen aan een locatie van uw keuze. Bijvoorbeeld, kunt u deze koppelen aan een specifieke organisatie-eenheid. U kan ook koppelen aan een specifieke groep computers die automatisch met Azure Active Directory registreren. Dit beleid instellen voor alle domein behoren Windows 10 en Windows Server 2016 computers in uw organisatie, het groepsbeleidsobject te koppelen aan het domein.

### <a name="download-windows-installer-packages-for-non-windows-10-computers"></a>Windows Installer-pakketten voor Windows 10 computers downloaden  

Registreren van een domein behoren computers met Windows 8.1, Windows 7, Windows Server 2012 R2, Windows Server 2008 R2 of Windows Server 2012 kunt u downloaden en installeren van deze bestanden van Windows Installer-pakket (.msi):

- [x64](http://download.microsoft.com/download/C/A/7/CA79FAE2-8C18-4A8C-A4C0-5854E449ADB8/Workplace_x64.msi)
- [x86](http://download.microsoft.com/download/C/A/7/CA79FAE2-8C18-4A8C-A4C0-5854E449ADB8/Workplace_x86.msi)

Het pakket implementeren met behulp van een distributiestelsel software zoals System Center Configuration Manager. Het pakket ondersteunt de opties standaard een installatie zonder toezicht met de parameter *stil* . System Center Configuration Manager 2016 biedt aanvullende voordelen van eerdere versies, zoals de mogelijkheid om bij te houden van de ingevulde registraties. Zie [System Center 2016](https://www.microsoft.com/en-us/cloud-platform/system-center)voor meer informatie.

Het installatieprogramma maakt een geplande taak op het systeem dat wordt uitgevoerd in de context van de gebruiker. De taak wordt gestart wanneer de gebruiker zich bij Windows aanmeldt. Het apparaat registreert de taak zonder waarschuwing met Azure Active Directory met de referenties van de gebruiker na verificatie via geïntegreerde Windows. De geplande taak, Ga naar **Microsoft** > **Lid van de werkplek**en vervolgens Ga naar de bibliotheek voor Taakplanner.



## <a name="next-steps"></a>Volgende stappen

- [Azure Active Directory voorwaardelijke toegang](active-directory-conditional-access.md)
