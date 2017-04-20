<properties
    pageTitle="Wat is de functie zelf aanmelden voor Azure? | Microsoft Azure"
    description="Een overzicht zelf aanmelden voor Azure, het beheren van de aanmeldingsprocedure en het nemen van een DNS-domeinnaam."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/23/2016"
    ms.author="curtand"/>


# <a name="what-is-self-service-signup-for-azure"></a>Wat is de functie zelf aanmelden voor Azure?

In dit onderwerp wordt uitgelegd de self-service aanmeldingsproces en hoe over een DNS-domeinnaam te nemen.  

## <a name="why-use-self-service-signup"></a>Waarom gebruikt u zelf aanmelden?

- Klanten ze sneller willen services ophalen.
- Voorstellen voor een service op basis van een e-mailbericht maken.
- Maken op basis van een e-mailadres aanmelden stromen die snel kunnen gebruikers met behulp van hun werk gemakkelijk te onthouden e-mail-aliassen identiteiten maken.
- Onbeheerde Azure mappen kunnen later in beheerde mappen worden gemaakt en opnieuw worden gebruikt voor andere services.

## <a name="terms-and-definitions"></a>Termen en definities

+ **Self-service aanmelden**: dit is de manier waarop een gebruiker zich aanmeldt voor een cloud-service en heeft een identiteit voor hen in Azure Active Directory (AD Azure) automatisch gemaakt op basis van hun e-maildomein.
+ **Onbeheerde Azure directory**: dit is de map waar de identiteit die is gemaakt. Een niet-beheerde map is een map die geen algemene beheerder heeft.
+ **E-mailadres geverifieerd gebruiker**: dit is een type gebruikersaccount in AD Azure. Een gebruiker met een identiteit die automatisch worden gemaakt na het aanmelden voor een aanbieding zelf staat bekend als een gebruiker e-mail wordt gecontroleerd. Een e-mailadres geverifieerd gebruiker gewone lid is van een map gelabeld met creationmethod = EmailVerified.

## <a name="user-experience"></a>Gebruikerservaring

Bijvoorbeeld, Stel dat een gebruiker wiens e-mailadres is Dan@BellowsCollege.com gevoelige bestanden via e-mail ontvangt. De bestanden zijn beveiligd door Azure Rights Management (Azure RMS). Maar Dan van organisatie, balg College, niet heeft aangemeld voor RMS Azure, noch is het Active Directory-RMS geïmplementeerd. In dit geval, kunt Dan aanmelden voor een gratis abonnement op RMS voor personen om de beveiligde bestanden lezen.

Als Dan de eerste gebruiker met een e-mailadres van de BellowsCollege.com aan te melden voor deze self-service aanbiedt, en vervolgens een niet-beheerde map wordt gemaakt voor BellowsCollege.com in Azure AD. Als andere gebruikers van het domein BellowsCollege.com voor deze aanbieding of een soortgelijke zelf aanbieden aanmelden, hebben ze ook e-mailadres geverifieerd gebruikersaccounts gemaakt in de map zonder begeleiding in Azure.

## <a name="admin-experience"></a>Admin-ervaring

Een beheerder die eigenaar is van de DNS-naam van een niet-beheerde Azure directory kunt overnemen of de map samenvoegen na het bewijs van eigendom. De volgende gedeelten wordt uitgelegd dat de admin-ervaring in meer detail, maar hier volgt een overzicht:

- Wanneer u via een niet-beheerde Azure directory, is simpelweg de globale beheerder van de niet-beheerde map zijn. Dit wordt soms een interne overname genoemd.
- Wanneer u een niet-beheerde map Azure samenvoegt, u de DNS-naam van de niet-beheerde map aan uw beheerde map Azure toevoegen en een toewijzing van gebruikers tot netwerkbronnen wordt gemaakt zodat gebruikers kunnen blijven toegang te krijgen tot de diensten zonder onderbreking. Dit wordt soms een externe overname genoemd.

## <a name="what-gets-created-in-azure-active-directory"></a>Wat wordt gemaakt in Azure Active Directory?

#### <a name="directory"></a>Directory

- Een map Azure Active Directory voor het domein wordt gemaakt, één map per domein.
- De map Azure AD heeft geen algemene beheerder.

#### <a name="users"></a>Gebruikers

- Voor elke gebruiker die zich aanmeldt, wordt een object gemaakt in de map Azure AD.
- Elk user-object is gemarkeerd als extern.
- Elke gebruiker wordt toegang verleend tot de service die ze aangemeld voor.

### <a name="how-do-i-claim-a-self-service-azure-ad-directory-for-a-domain-i-own"></a>Hoe claim een self-service Azure AD Active directory voor een domein dat ik de eigenaar?

U kunt aanspraak maken op een self-service Azure AD Active directory door het valideren van het domein. Domeinverificatie bewijst dat u eigenaar bent van het domein met DNS-records maken.

Er zijn twee manieren een DNS-overname van een Azure AD-map:

- interne overname (Admin een onbeheerde Azure directory ontdekt en wil omzetten in een beheerde map)
- externe overname (Admin probeert een nieuw domein toevoegen aan hun beheerde Azure directory)

Mogelijk willen verifiëren dat u eigenaar bent van een domein omdat u via een niet-beheerde map nemen nadat een gebruiker de aanmelding zelf uitgevoerd of u mogelijk worden een nieuw domein aan een bestaande beheerde map toevoegen. Bijvoorbeeld, u een domein met de naam contoso.com hebt en u wilt toevoegen een nieuw domein met de naam contoso.co.uk of contoso.uk.

## <a name="what-is-domain-takeover"></a>Wat is domein-overname?  

In deze sectie wordt beschreven hoe om te verifiëren dat u eigenaar bent van een domein

### <a name="what-is-domain-validation-and-why-is-it-used"></a>Wat is validatie van het domein en waarom wordt het gebruikt?

Om bewerkingen uitvoeren op een map, Azure AD is vereist dat u eigenaar van het DNS-domein worden gevalideerd.  Validatie van het domein, kunt u aanspraak maken op de map en een bevordering van de map zelf naar een beheerde map of de map zelf samenvoegen met een bestaande beheerde map.

## <a name="examples-of-domain-validation"></a>Voorbeelden van validatie van domein

Er zijn twee manieren een DNS-overname van een map:

+ interne overname (bijvoorbeeld een admin een map zelf, zonder begeleiding ontdekt en wil omzetten in een beheerde map)
+ externe overname (bijvoorbeeld een beheerder wil toevoegen een nieuw domein aan een beheerde map)

### <a name="internal-takeover---promote-a-self-service-unmanaged-directory-to-be-a-managed-directory"></a>Interne overname - bevordering van een directory zelf, zonder begeleiding een beheerde map

Wanneer u interne overname doet, wordt de map uit een niet-beheerde map geconverteerd naar een beheerde map. U moet DNS-domein naam validatie, waar u een MX-record of een TXT-record in de DNS-zone maken te voltooien. Actie:

+ Eigenaar van het domein worden gevalideerd
+ Kunt u de directory wordt beheerd
+ Maakt u het globale beheer van de directory

Stel dat u dat een IT-beheerder van de school balg ontdekt dat gebruikers van de school voor self-service aanbiedingen hebt aangemeld. De geregistreerde eigenaar van de DNS-namen, BellowsCollege.com, kan de IT-beheerder eigendom van de DNS-naam in Azure valideren en de niet-beheerde map vervolgens overnemen. De map wordt dan een beheerde map en de IT-beheerder de globale beheerdersrol voor de map BellowsCollege.com toegewezen.

### <a name="external-takeover---merge-a-self-service-directory-into-an-existing-managed-directory"></a>Externe overname - een self-service map samenvoegen met een bestaande beheerde map

In een externe overname hebt u al een beheerde map en u wilt dat alle gebruikers en groepen uit een niet-beheerde directory deel te nemen aan deze beheerde map, in plaats van eigen twee afzonderlijke mappen.

Als een beheerder van een beheerde map u een domein toevoegt en dat domein gebeurt met een niet-beheerde map gekoppeld zijn.

Bijvoorbeeld, Stel u een IT-beheerder bent en u hebt al een beheerde map voor Contoso.com, een domeinnaam die is geregistreerd in uw organisatie. U ontdekt die gebruikers in uw organisatie hebt uitgevoerd zelf ondertekenen omhoog voor een aanbieding via e-maildomeinnamen user@contoso.co.uk, die een andere domeinnaam waarin uw organisatie is. Deze gebruikers hebben accounts in een niet-beheerde map voor contoso.co.uk.

U wilt niet dat voor het beheer van twee afzonderlijke mappen, zodat u de niet-beheerde map voor contoso.co.uk samenvoegen met de bestaande beheerde IT-map voor contoso.com.

Externe overname volgt dezelfde DNS-validatieproces als interne overname.  Dat verschil: gebruikers en services worden opnieuw toegewezen aan de directory beheerde IT.

#### <a name="whats-the-impact-of-performing-an-external-takeover"></a>Wat is de impact van het uitvoeren van een externe overname?

Een toewijzing van gebruikers tot netwerkbronnen wordt met een externe overname gemaakt zodat gebruikers toegang krijgen tot de diensten zonder onderbreking kunnen blijven. Veel toepassingen, met inbegrip van RMS voor individuele gebruikers, het toewijzen van gebruikers tot bronnen goed verwerken en gebruikers kunnen toegang krijgen tot deze services zonder wijziging gaan. Als een toepassing het toewijzen van gebruikers tot netwerkbronnen niet daadwerkelijk verwerkt, worden expliciet externe overname geblokkeerd als u wilt voorkomen dat gebruikers een slechte ervaring.

#### <a name="directory-takeover-support-by-service"></a>openbaar overnamebod ondersteuning van Directory service

Momenteel ondersteunen de volgende services overname:

- RMS


De volgende services worden binnenkort ondersteunen overname:

- PowerBI

Het volgende niet doen en vereisen extra admin actie voor het migreren van gegevens van de gebruiker na een externe overname.

- SharePoint/OneDrive


## <a name="how-to-perform-a-dns-domain-name-takeover"></a>Het uitvoeren van een DNS-domein naam overname

Hebt u een aantal opties voor het uitvoeren van een validatie van het domein (en een overname te doen als je wilt):

1.  Azure Management Portal

    Een overname wordt geactiveerd door de toevoeging van een domein te doen.  Als het domein al een map bestaat, hebt u de optie voor het uitvoeren van een externe overname.

    Log in om de Azure portal met uw referenties.  Ga naar de bestaande map en vervolgens aan **domein toevoegen**.

2.  Office 365

    U kunt de opties op de pagina [Manage domains](https://support.office.com/article/Navigate-to-the-Office-365-Manage-domains-page-026af1f2-0e6d-4f2d-9b33-fd147420fac2/) in Office 365 werken met uw domeinen en DNS-records. Zie [controleren of uw domein in Office 365](https://support.office.com/article/Verify-your-domain-in-Office-365-6383f56d-3d09-4dcb-9b41-b5f5a5efd611/).

3.  Windows PowerShell

    De volgende stappen zijn vereist voor het uitvoeren van een validatie met behulp van Windows PowerShell.

    Stap    |   De cmdlet gebruiken
    ------- | -------------
    Een referentie-object maken | Get-referentie
    Verbinding maken met Azure AD | Verbinding maken met MsolService
    een lijst met domeinen   | Get-MsolDomain
    Maak een uitdaging  | Get-MsolDomainVerificationDns
    DNS-record maken   | Doet u dit op uw DNS-server
    Controleer of de uitdaging    | Bevestiging MsolEmailVerifiedDomain

Bijvoorbeeld:

1. Verbinding maken met Azure AD met de referenties die werden gebruikt om te reageren op het aanbod zelf: import-module MSOnline $msolcred = get-credential connect-msolservice-$msolcred van referenties

2. Een lijst met domeinen opvragen:

    Get-MsolDomain

3. Voer de cmdlet Get-MsolDomainVerificationDns om een uitdaging te maken:

    Get MsolDomainVerificationDns domeinnaam – *your_domain_name* – DnsTxtRecord-modus

    Bijvoorbeeld:

    Get-MsolDomainVerificationDns – domeinnaam contoso.com – DnsTxtRecord modus

4. De waarde die wordt geretourneerd door deze opdracht (challenge) kopiëren.

    Bijvoorbeeld:

    MS = 32DD01B82C05D27151EA9AE93C5890787F0E65D9

5. Maak een DNS txt-record met de waarde die u in de vorige stap hebt gekopieerd in de openbare DNS-domeinnaamruimte.

    De voor deze record is de naam van het bovenliggende domein, dus als u deze bronrecord maakt met behulp van de DNS-functie van Windows Server, laat de plakken Record naam leeg en alleen de waarde in het tekstvak

6. Voer de cmdlet bevestigen MsolDomain om te controleren of de uitdaging:

    Controleer MsolEmailVerifiedDomain - domeinnaam *your_domain_name*

    bijvoorbeeld:

    Controleer MsolEmailVerifiedDomain - domeinnaam contoso.com

Een geslaagde uitdaging keert u terug naar de opdrachtprompt zonder een fout.

## <a name="how-do-i-control-self-service-settings"></a>Hoe bepaal ik de instellingen zelf?

Beheerders hebben vandaag twee besturingselementen zelf. Ze kunnen bepalen:

- Of gebruikers deelnemen aan de directory via e-mail.
- Gebruikers kunnen de zelf of licentie voor toepassingen en services.


### <a name="how-can-i-control-these-capabilities"></a>Hoe kan ik deze mogelijkheden bepalen?

Een beheerder kan deze functies met deze cmdlet Set-MsolCompanySettings AD Azure parameters configureren:

+ **AllowEmailVerifiedUsers** bepaalt of een gebruiker kan maken of deelnemen aan een niet-beheerde map. Als u deze parameter op $false instelt, hebben geen e-mailadres geverifieerd gebruikers kunnen deelnemen aan de directory.
+ **AllowAdHocSubscriptions** regelt de mogelijkheid van gebruikers zelf ondertekenen omhoog. Als u deze parameter op $false instelt, kunnen geen gebruikers aanmelden zelf uitvoeren.


### <a name="how-do-the-controls-work-together"></a>Hoe werken de besturingselementen samen?

Deze twee parameters kunnen in combinatie worden gebruikt voor het definiëren van meer controle over het zelf ondertekenen van. Bijvoorbeeld de volgende opdracht kunnen gebruikers zelf ondertekenen, maar alleen wordt uitgevoerd als gebruikers al een account in Azure AD hebt (met andere woorden, gebruikers hoeven een e-mailadres geverifieerd account moet worden gemaakt niet uitvoeren zelf ondertekenen omhoog):

    Set-MsolCompanySettings -AllowEmailVerifiedUsers $false -AllowAdHocSubscriptions $true

Het volgende stroomdiagram beschrijft verschillende combinaties voor deze parameters en de resulterende voorwaarden voor de map en het teken zelf omhoog.

![][1]

Zie voor meer informatie en voorbeelden van het gebruik van deze parameters [Set MsolCompanySettings](https://msdn.microsoft.com/library/azure/dn194127.aspx).

## <a name="see-also"></a>Zie ook

-  [Het installeren en configureren van Azure PowerShell](../powershell-install-configure.md)

-  [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx)

-  [Azure Cmdlet referentie](https://msdn.microsoft.com/library/azure/jj554330.aspx)

-  [Set MsolCompanySettings](https://msdn.microsoft.com/library/azure/dn194127.aspx)

<!--Image references-->
[1]: ./media/active-directory-self-service-signup/SelfServiceSignUpControls.png
