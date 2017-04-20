<properties
    pageTitle="Azure AD verbinden: Gebruiker aanmelden | Microsoft Azure"
    description="Azure AD verbinden gebruiker inloggen voor aangepaste instellingen."
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
    ms.date="10/16/2016"
    ms.author="billmath"/>



# <a name="azure-ad-connect-user-sign-on-options"></a>Azure AD verbinding gebruiker aanmelden op Opties

Azure AD Connect kan uw gebruikers aan te melden voor zowel cloud als op ruimten resources met dezelfde wachtwoorden. Basisbegrippen voor elke identiteitsmodel kunt u Kies de identiteit die dat u wilt gebruiken voor het aanmelden op Azure AD beschreven.

Als u al bekend bent met Azure AD identiteitsmodel voor meer informatie over een specifieke methode, net Klik hieronder op het betreffende onderwerp.

* [Password Sync](#password-synchronization)
* [Federatieve eenmalige aanmelding (met AD FS)](#federation-using-a-new-or-existing-ad-fs-in-windows-server-2012-r2-farm)


## <a name="choosing-a-user-sign-in-method"></a>Een gebruiker aanmelden methode kiezen
Voor de meeste organisaties die willen gebruiker zich voor Office 365 aanmeldt, bronnen, op basis van SaaS-toepassingen en andere Azure AD het standaard wachtwoord synchronisatieoptie wordt aanbevolen.
Sommige organisaties hebben echter bepaalde redenen voor het gebruik van een federatieve teken optie zoals AD FS.  Deze omvatten:

- Uw organisatie al gebruikmaakt van AD FS of een 3de partij federation provider geïmplementeerd
- Het beveiligingsbeleid verbiedt wachtwoord-hashes naar de cloud synchroniseren
- Dat de gebruikers naadloze SSO ondervinden (zonder extra wachtwoord wordt gevraagd) vereisen als machines op het bedrijfsnetwerk toegang tot cloud resources uit domein lid
- Sommige specifieke mogelijkheden voor die AD FS is vereist
    - Over lokalen meerledige verificatie met behulp van een derde provider of smartcards (Klik hier voor informatie over andere MVR gesloten leveranciers voor AD FS in Windows Server 2012 R2)
    - Voorzieningen van Active Directory-integratie zoals AD wachtwoord- en uren beleid of zachte accountvergrendeling
    - Voorwaardelijke toegang tot beide op gebouwen en resources met de registratie of Azure AD join Intune MDM beleid cloud

### <a name="password-synchronization"></a>Wachtwoordsynchronisatie
Met Wachtwoordsynchronisatie-hashes van gebruikerswachtwoorden van Active Directory op ruimten gesynchroniseerd met Azure AD.  Wanneer wachtwoorden worden gewijzigd of opnieuw ingesteld voor de lokalen, worden de nieuwe wachtwoorden onmiddellijk naar Azure AD gesynchroniseerd zodat uw gebruikers altijd hetzelfde wachtwoord voor cloud resources gebruiken kunnen als op locatie.  De wachtwoorden worden nooit verzonden naar Azure AD noch in Azure AD in leesbare tekst opgeslagen.
Wachtwoordsynchronisatie kan worden gebruikt met de terugneming wachtwoord self Servicewachtwoord opnieuw instellen in Azure AD inschakelen.

<center>![Wolk](./media/active-directory-aadconnect-user-signin/passwordhash.png)</center>

[Meer informatie over Wachtwoordsynchronisatie](active-directory-aadconnectsync-implement-password-synchronization.md)


### <a name="federation-using-a-new-or-existing-ad-fs-in-windows-server-2012-r2-farm"></a>Federatie met een nieuwe of bestaande AD FS in de farm Windows Server 2012 R2
Federatieve teken op kunnen uw gebruikers aanmelden bij Azure AD gebaseerd services met hun wachtwoorden op gebouwen en, terwijl op het bedrijfsnetwerk, zonder hun wachtwoord opnieuw invoeren.  De optie federation met AD FS kunt u implementeert een nieuwe of een bestaande AD FS in de farm Windows Server 2012 R2 opgeven.  Als u een bestaande serverfarm opgeven, configureren Azure AD verbinden de vertrouwensrelatie tussen uw bedrijf en Azure AD zodat uw gebruikers kunnen aanmelden.

<center>![Wolk](./media/active-directory-aadconnect-user-signin/federatedsignin.png)</center>

#### <a name="to-deploy-federation-with-ad-fs-in-windows-server-2012-r2-you-will-need-the-following"></a>Als u wilt implementeren Federation met AD FS in Windows Server 2012 R2, moet u de volgende

Als u een nieuwe farm implementeert:

- Een Windows Server 2012 R2 server voor de federation-server.
- Een Windows Server 2012 R2 server voor de toepassing Web Proxy.
- een pfx-bestand met een SSL-certificaat voor de beoogde federation-service-naam, bijvoorbeeld fs.contoso.com.

Als u een nieuwe serverfarm implementeren of een bestaande serverfarm gebruikt:

- Referenties voor de lokale beheerder van de federation-servers.
- Referenties van lokale beheerder op een werkgroep (niet van het domein lid) waarop u van plan bent voor de implementatie van de functie Web Application Proxy servers.
- De computer waarop u de wizard uitvoert moet verbinding kunnen maken met andere computers waarop u wilt installeren van AD FS of Web Application Proxy via Windows Remote Management.

[Eenmalige aanmelding met AD FS configureren](./aad-connect/active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs) 

#### <a name="sign-on-using-an-earlier-version-of-ad-fs-or-a-third-party-solution"></a>Aanmelden met een eerdere versie van AD FS of een derde oplossing

Als u over het gebruik van een eerdere versie van AD FS (zoals AD FS 2.0) of een federation-serviceprovider van derden cloud teken al hebt geconfigureerd, kunt u overslaan gebruiker inloggen configuratie via Azure AD verbinden.  Hiermee kunt u de laatste synchronisatie en andere mogelijkheden van Azure AD verbinden terwijl u ook uw bestaande oplossing voor aanmelding op.

[Azure AD derden federation compatibility lijst](./active-directory-aadconnect-federation-compatibility.md)

## <a name="user-sign-in-and-user-principal-name-upn"></a>Aanmelden van gebruiker en de UPN (user Principal name)

### <a name="understanding-user-principal-name"></a>Wat zijn UPN-naam

In Active Directory is het standaard-UPN-achtervoegsel de DNS-naam van het domein waarin de gebruikersaccount gemaakt. In de meeste gevallen is dit de naam van het domein geregistreerd als het domein van de onderneming op het Internet. U kunt echter meer UPN-achtervoegsels die met Active Directory: domeinen en vertrouwensrelaties toevoegen.

De UPN van de gebruiker van de indeling is username@domai. Bijvoorbeeld voor een active directory contoso.com gebruiker John mogelijk UPN john@contoso.com. De UPN van de gebruiker is gebaseerd op RFC 822. Hoewel de UPN- en e-mailadres dezelfde indeling delen, de waarde van de UPN voor een gebruiker al dan niet gelijk is aan het e-mailadres van de gebruiker.

### <a name="user-principal-name-in-azure-ad"></a>UPN-naam in Azure AD

Azure AD verbinding maken met wizard wordt het kenmerk userPrincipalName gebruiken of kunt u het kenmerk (in aangepaste installatie) in Azure AD op ruimten worden gebruikt als de UPN-naam opgeven. Dit is de waarde die wordt gebruikt voor het aanmelden bij AD Azure. Als de waarde van het kenmerk van de principal-naam van gebruiker niet met een gecontroleerde domein in Azure AD overeen komt, Azure AD wordt vervangen door een standaardwaarde. onmicrosoft.com-waarde.

Elke map in Azure Active Directory wordt geleverd met een ingebouwde domeinnaam in het formulier contoso.onmicrosoft.com waarmee u aan de slag met Azure of andere Microsoft-services. U kunt verbeteren en vereenvoudigen aanmelden met behulp van aangepaste domeinen. Voor meer informatie over aangepaste domein Lees namen in Azure AD en het controleren van een domein, [uw aangepaste domeinnaam naar Azure Active Directory toevoegen](active-directory-add-domain.md#add-your-custom-domain-name-to-azure-active-directory)

## <a name="azure-ad-sign-in-configuration"></a>Azure AD-in configuratie

### <a name="azure-ad-sign-in-configuration-with-azure-ad-connect"></a>Azure AD-in de configuratie met Azure AD verbinden
Azure AD-in ervaring is afhankelijk van of Azure AD kunnen overeenkomen met het UPN-achtervoegsel van een gebruiker wordt gesynchroniseerd met een van de aangepaste domeinen in Active directory AD Azure gecontroleerd. Azure AD Connect biedt hulp bij het configureren van AD Azure-in instellingen-in gebruikerservaring in de cloud is vergelijkbaar met de ervaring van gebouwen. Azure AD Connect vindt u de UPN-achtervoegsels gedefinieerd voor de domeinen en probeert deze in Azure AD overeenkomen met een aangepast domein en kunt u met de actie die moet worden ondernomen.
Azure AD-in pagina geeft een overzicht van UPN-achtervoegsels die zijn gedefinieerd voor de Active Directory op de ruimten en de bijbehorende status tegen elke achtervoegsel. De mogelijke statussen zijn de volgende:

| Staat | Beschrijving | Actie vereist |
|:----|:----|:----|
|Gecontroleerd| Azure AD verbinden gevonden dat een bijbehorend domein geverifieerd in Azure AD. Alle gebruikers voor dit domein kunnen aanmelden met hun referenties op ruimten| Er is geen actie vereist |
|Niet geverifieerd| Azure AD verbinding maken met een overeenkomende aangepaste domein kan vinden in Azure AD, maar wordt niet gecontroleerd. UPN-achtervoegsel van de gebruikers van dit domein wordt gewijzigd naar standaard. onmicrosoft.com achtervoegsel na synchronisatie als het domein niet wordt gecontroleerd. | Controleer of het aangepaste domein in Azure AD. [Meer informatie](./active-directory-add-domain.md#verify-the-domain-name-with-azure-ad) |  
|Niet toegevoegd| Azure AD verbinding maken met een aangepast domein overeenkomen met het UPN-achtervoegsel niet gevonden. UPN-achtervoegsel van de gebruikers van dit domein wordt gewijzigd naar standaard. onmicrosoft.com als het domein is niet toegevoegd en verifeid in Azure.| Toevoegen en controleer of een aangepast domein voor [Meer informatie](./active-directory-add-domain.md) het UPN-achtervoegsel|

Azure AD-aanmeldingspagina staan de UPN-suffix(s) voor Active Directory op de ruimten en de bijbehorende aangepaste domein in Azure AD met de huidige verificatiestatus is gedefinieerd. Selecteer nu het kenmerk voor een UPN-naam op de aanmeldingspagina **Azure AD** in aangepaste installatie.

![Azure AD-aanmeldingspagina](./media/active-directory-aadconnect-user-signin/custom_azure_sign_in.png)

U kunt klikken op Vernieuwen om de meest recente status van de aangepaste domeinen opnieuw ophalen uit Azure Active Directory.

###<a name="selecting-attribute-for-user-principal-name-in-azure-ad"></a>Kenmerk voor UPN-naam selecteren in Azure AD

UserPrincipalName - het kenmerk userPrincipalName is het kenmerk gebruikers werken gaan als ze inloggen AD Azure en Office 365. De domeinen gebruikt, ook wel bekend als het UPN-achtervoegsel, moeten worden geverifieerd in Azure AD voordat de gebruikers worden gesynchroniseerd. Het is raadzaam om de standaard kenmerk userPrincipalName. Als dit kenmerk niet routeerbaar is en kan niet worden gecontroleerd, dan is het mogelijk aan een ander kenmerk selecteren, bijvoorbeeld e-mail, als het kenmerk met de aanmeldings-ID. Dit staat bekend als alternatieve-ID. De waarde van het kenmerk ID alternatieve moet voldoen aan de standaard RFC822. Een alternatieve ID kan worden gebruikt met wachtwoord Single Sign On (SSO) en Federatie SSO als oplossing voor het aanmelden.

>[AZURE.NOTE] Een alternatieve ID is niet compatibel met alle Office 365 werkbelasting. Zie [Alternatieve aanmeldings-ID configureren](https://technet.microsoft.com/library/dn659436.aspx)voor meer informatie.

#### <a name="different-custom-domain-states-and-effect-on-azure-sign-in-experience"></a>De lidstaten verschillende aangepaste domein en het effect op Azure-in-ervaring
Het is heel belangrijk om te begrijpen van de relatie tussen de Staten aangepaste domein in de directory Azure AD en UPN-achtervoegsels gedefinieerd op gebouwen. Laat ons doorlopen voor de verschillende mogelijke Azure-in ervaringen bij het instellen van Azure AD Processtappen verbinden met sycnhronization.

De informatie hieronder laat ons wordt ervan uitgegaan dat we betrokken zijn bij het UPN-achtervoegsel contoso.com die wordt gebruikt in de map op de locatie als onderdeel van de UPN, bijvoorbeeld user@contoso.com.

###### <a name="express-settings--password-synchronization"></a>Express instellingen / Wachtwoordsynchronisatie
| Staat         | Effect op de gebruikerservaring Azure aanmelden |
|:-------------:|:----------------------------------------|
| Niet toegevoegd | In dit geval is geen aangepaste domein contoso.com in Azure AD directory toegevoegd. Gebruikers met een UPN-op-locatie met achtervoegsel @contoso.com, worden niet meer met de UPN in lokalen aanmelden bij Azure. In plaats daarvan hebben ze een nieuwe UPN die ze van Azure AD door het standaard Azure AD Directory gebruiken. Bijvoorbeeld, als u bij het synchroniseren van gebruikers Azure AD directory azurecontoso.onmicrosoft.com vervolgens de gebruiker op gebouwen user@contoso.com de UPN-krijgtuser@azurecontoso.onmicrosoft.com|
| Niet geverifieerd | In dit geval hebben we een aangepaste domein contoso.com toegevoegd in Azure AD directory, maar deze nog niet is geverifieerd. Als u verder gaat met gebruikers zonder te controleren van het domein synchroniseren, vervolgens de gebruikers krijgen een nieuwe UPN door Azure AD, net zoals dat het geval in het scenario 'Niet toegevoegd was'.|
| Gecontroleerd | In dit geval hebben we een aangepaste domein contoso.com al toegevoegd en geverifieerd in Azure advertentie voor het UPN-achtervoegsel. Gebruikers kunnen gebruiken hun op ruimten UPN-naam, bijvoorbeeld user@contoso.com, om aan te melden voor Azure nadat ze zijn gesynchroniseerd met Azure AD|

###### <a name="ad-fs-federation"></a>AD FS-federatie
U kunt een federatie met standaard maken. domein in Azure AD onmicrosoft.com of een niet-geverifieerde aangepaste domein in Azure AD. Wanneer u de wizard Azure AD verbinding maken als u een niet-geverifieerde domein maken krijgt een federatie met vervolgens Azure AD verbinding u benodigde records worden gemaakt waarbij de DNS voor het domein wordt gehost. Zie voor meer informatie [hier](active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation).

Als u aanmelden met de optie gebruiker als "Federatie met AD FS" hebt geselecteerd, hebt u een aangepast domein om door te gaan met het maken van een Federatie in Azure AD. Voor onze discussie betekent dit dat we moeten een aangepaste domein contoso.com in Azure AD directory toegevoegd.

| Staat         | Effect op de gebruikerservaring Azure aanmelden |
|:-------------:|:----------------------------------------|
| Niet toegevoegd | In dit geval Azure AD verbinden geen gevonden een overeenkomende aangepaste domein voor het UPN-achtervoegsel contoso.com in Azure AD directory. U moet een aangepast domein contoso.com toevoegen als u gebruikers om aan te melden met behulp van AD FS met de UPN in bedrijfsruimten zoals user@contoso.com.|
| Niet geverifieerd | In dit geval krijgt Azure AD verbinding u met de juiste informatie over hoe u kunt controleren of uw domein in een later stadium|
| Gecontroleerd | In dit geval gaat u verder met de configuratie zonder verdere actie|  

## <a name="changing-user-sign-in-method"></a>Methode voor aanmelden wijzigen

Kunt u de gebruiker aanmelden methode van Federation wachtwoord synchronisatie met de avaialble taken in Azure AD verbinden na de initiële configuratie van Azure AD verbinding maken met de wizard. De Azure AD verbinding maken met wizard opnieuw uitvoeren en u krijgt een lijst met taken die u kunt uitvoeren. **Wijzigen gebruiker aanmelden** selecteren uit de lijst met taken. 

![Aanmelden van gebruiker wijzigen"](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

Op de volgende pagina, wordt u gevraagd de referenties voor Azure advertentie opgeven.

![Verbinding maken met Azure AD](./media/active-directory-aadconnect-user-signin/changeusersignin2.png)

Selecteer op de pagina **gebruiker aanmelden** **Wachtwoordsynchronisatie**. Hiermee wijzigt u de map naar een beheerde federatieve.

![Verbinding maken met Azure AD](./media/active-directory-aadconnect-user-signin/changeusersignin3.png)

>[AZURE.NOTE] Als u tijdelijk overschakelen naar Wachtwoordsynchronisatie maakt, controleert u de **gebruikersaccounts worden niet geconverteerd**. Federatieve niet controleren op de optie zal leiden tot een conversie van elke gebruiker en kan het enkele uren duren.
  
## <a name="next-steps"></a>Volgende stappen
Meer informatie over de [integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md).

Meer informatie over [Azure AD verbinden: concepten ontwerpen](active-directory-aadconnect-design-concepts.md)


