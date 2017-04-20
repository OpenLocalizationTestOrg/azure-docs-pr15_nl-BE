<properties 
    pageTitle="Configureren van eenmalige aanmelding voor toepassingen die niet in de galerie met Azure Active Directory application | Microsoft Azure" 
    description="Meer informatie over hoe self-service verbonden apps naar Azure Active Directory met behulp van SAML en eenmalige aanmelding op basis van wachtwoorden" 
    services="active-directory" 
    authors="asmalser-msft"  
    documentationCenter="na" manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="02/09/2016" 
    ms.author="asmalser" />

#<a name="configuring-single-sign-on-to-applications-that-are-not-in-the-azure-active-directory-application-gallery"></a>Eenmalige aanmelding voor toepassingen die niet in de galerie met Azure Active Directory-toepassing configureren

Dit artikel is een voorziening waarmee beheerders voor het configureren van eenmalige aanmelding voor toepassingen die niet aanwezig in de Azure Active Directory app galerie *-code te schrijven*. Deze functie is vrijgegeven uit technische preview op de 18e November 2015 en is opgenomen in [Active Directory-Premium Azure](active-directory-editions.md). Als u in plaats daarvan ontwikkelaar richtsnoeren over het integreren van aangepaste toepassingen met Azure Active Directory via de programmacode zoekt, Zie [Scenario's voor Azure advertentie voor verificatie](active-directory-authentication-scenarios.md).

De galerie Azure Active Directory-toepassingen biedt een overzicht van de toepassingen waarvan bekend is dat een vorm van eenmalige aanmelding met Azure Active Directory ondersteunen, zoals beschreven in [dit artikel](active-directory-appssoaccess-whatis.md). Als u (een IT specialist of system integrator in uw organisatie) hebt gevonden dat de toepassing die u wilt verbinden, u kunt aan de slag door de instructies stapsgewijs weergegeven in de portal management Azure eenmalige aanmelding inschakelen.

Klanten met [Azure Active Directory Premium](active-directory-editions.md) licenties krijgen ook deze extra mogelijkheden:

* Self-service integratie van elke toepassing die SAML 2.0 id-providers (SP gestart of geïnitieerd IdP ondersteunt)
* Self-service integratie van een webtoepassing die met een op basis van HTML-aanmeldingspagina met [eenmalige aanmelding op basis van wachtwoorden](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)
* De verbinding zelf van toepassingen die gebruikmaken van de SCIM-protocol voor de gebruiker provisioning ([hieronder beschreven](active-directory-scim-provisioning.md))
* Koppelingen toevoegen aan een toepassing in de [Office 365-app starten](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) of het [deelvenster Azure AD-toegang](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users)

Het kan hierbij niet alleen SaaS-toepassingen die u gebruikt, maar aan boord zijn niet nog is over-gegaan naar de galerie met Azure AD toepassing, maar derden webtoepassingen die uw organisatie is geïmplementeerd op servers die u, in de cloud of op locatie beheert.

Verbindingspunten op standaarden gebaseerde bieden deze mogelijkheden, ook bekend als *app integratie sjablonen*voor toepassingen die SAML, SCIM of op formulieren gebaseerde verificatie ondersteunt en flexibele opties en instellingen voor compatibiliteit met een groot aantal toepassingen. 

##<a name="adding-an-unlisted-application"></a>Een niet-genoteerde toepassing toe te voegen

Verbinding van een toepassing met behulp van een sjabloon app-integratie wordt in de Azure management portal met de beheerdersaccount Azure Active Directory ondertekenen en blader naar de **Active Directory > [map] > toepassingen** sectie, selecteert u **toevoegen**en **toepassing uit de galerie toevoegen**. 

![][1]

In de galerij app kunt u toevoegen een nieuw app met behulp van de **aangepaste** categorie aan de linkerkant of door te klikken de **niet-genoteerde toepassing toevoegen** die wordt weergegeven in de zoekresultaten als de gewenste app is niet gevonden. Na het invoeren van een naam voor uw toepassing, kunt u de opties voor single sign-on en het gedrag. 

**Snelle tip**: beste, gebruikt u de zoekfunctie om te controleren om te zien als de toepassing al in de galerie van toepassing bestaat. Als de toepassing wordt gevonden en de beschrijving wordt vermeld 'eenmalige aanmelding' en vervolgens de toepassing wordt al ondersteund voor de federatieve eenmalige aanmelding. 

![][2]

Een toepassing toevoegen op deze manier biedt een zeer vergelijkbare ervaring bieden een vooraf geïntegreerde toepassingen beschikbaar. Selecteer **Configureren van eenmalige aanmelding**. Het volgende scherm geeft de volgende drie opties voor het configureren van eenmalige, die in de volgende secties worden beschreven.

![][3]


##<a name="azure-ad-single-sign-on"></a>Azure AD Single Sign-On

Selecteer deze optie om op basis van SAML-verificatie configureren voor de toepassing. Hiervoor is vereist dat de toepassingsondersteuning SAML 2.0 en u informatie te over het gebruik van de SAML-mogelijkheden van de toepassing verzamelen dient voordat u verdergaat. Nadat u hebt **volgende**, wordt u gevraagd op te geven van drie verschillende URL's die overeenkomen met de SAML-eindpunten voor de toepassing. 

![][4]
 
Dit zijn:

* **Teken in de URL (SP geïnitieerde alleen)** – wanneer de gebruiker om aan te melden voor deze toepassing gaat. Als de toepassing is geconfigureerd voor het uitvoeren van service provider geïnitieerde eenmalige op, vervolgens wanneer een gebruiker naar deze URL navigeert, doet de dienstverlener de nodige omleiding op Azure AD worden geverifieerd en het aanmelden van de gebruiker in. Als dit veld is ingevuld, vervolgens Azure AD gebruikt deze URL om de toepassing van Office 365 en Azure AD toegang te starten. Als dit veld ommited is en vervolgens AD Azure in plaats daarvan identiteitsprovider uitvoert-initiated teken op als de toepassing is gestart vanuit Office 365, Azure AD Access het deelvenster of de Azure AD eenmalige aanmelding URL (copiable op het tabblad Dashboard).

* **Uitgever URL** - de URL moet de toepassing voor welke eenmalige unieke identificatie op uitgever wordt geconfigureerd. Dit is de waarde die Azure AD teruggestuurd naar de toepassing als de parameter voor de **doelgroep** van het SAML-token wordt en de toepassing naar verwachting zal valideren. Deze waarde wordt ook weergegeven als de **Entiteit-ID** in de SAML-metagegevens die door de toepassing. SAML-documentatie van de toepassing voor meer informatie over wat het entiteit-ID of publiek waarde is. Hieronder volgt een voorbeeld van hoe de doelgroep URL wordt weergegeven in het SAML-token teruggestuurd naar de toepassing:

```
    <Subject>
    <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:unspecificed">chad.smith@example.com</NameID>
        <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />
      </Subject>
      <Conditions NotBefore="2014-12-19T01:03:14.278Z" NotOnOrAfter="2014-12-19T02:03:14.278Z">
        <AudienceRestriction>
          <Audience>https://tenant.example.com</Audience>
        </AudienceRestriction>
      </Conditions>
```

* **Antwoord URL** - de URL van het antwoord is waar de toepassing wordt verwacht te ontvangen van het SAML-token. Dit is ook de **bevestiging consument Service (ACS) URL**genoemd. Controleer de SAML-documentatie voor meer informatie over wat haar token antwoord SAML-URL of ACS-URL is van de toepassing.
 Nadat u deze hebt ingevoerd, klikt u op **volgende** om door te gaan naar het volgende scherm. Dit scherm bevat informatie over wat er worden geconfigureerd op de kant van de toepassing moet om het te accepteren van een SAML-token uit Azure Active Directory. 

![][5]

Waarden die nodig zijn afhankelijk van de toepassing, dus controleer SAML-documentatie van de toepassing voor meer informatie. De **Aanmelding** en **afmeldingstijden** service URL beide hetzelfde eindpunt, wordt het eindpunt SAML afhandelen van aanvragen voor uw exemplaar van AD Azure omzetten. De URL van de uitgevende instelling is de waarde die wordt weergegeven als de 'uitgever' in het SAML-token verleend aan de toepassing. 

Nadat de toepassing is geconfigureerd, klikt u op de knop **volgende** en vervolgens op het **volledig** op het dialoogvenster te sluiten. 

##<a name="assigning-users-and-groups-to-your-saml-application"></a>Gebruikers en groepen toewijzen aan de SAML-aanvraag 

Zodra uw toepassing is geconfigureerd als u wilt gebruiken een op SAML gebaseerde identiteitsprovider Azure AD, is het bijna klaar om te testen. Azure AD verleent als een beveiligingscontrole niet een token en hen aan te melden in de toepassing, tenzij met Azure AD toegang is verleend. Gebruikers kunnen toegang verleend rechtstreeks of via een groep die een lid van zijn. 

Als een gebruiker of groep toewijzen aan uw toepassing, klikt u op **Gebruikers toewijzen** . Selecteer de gebruiker of groep die u wilt toewijzen en selecteer vervolgens de knop **toewijzen** . 

![][6]

Een gebruiker toewijzen kunt Azure AD er een token voor de gebruiker, evenals een tegel voor deze toepassing moet worden weergegeven in het deelvenster voor toegang van de gebruiker wordt veroorzaakt. Een toepassing naast elkaar wordt ook weergegeven in de Office 365-toepassing starten als de gebruiker Office 365. 

U kunt een logo naast elkaar voor de toepassing met de knop **Logo uploaden** in het tabblad **configureren** voor de toepassing uploaden. 

###<a name="customizing-the-claims-issued-in-the-saml-token"></a>De vorderingen in het SAML-token verleend aanpassen 

Wanneer een gebruiker wordt geverifieerd aan de toepassing, verleent Azure AD SAML-token App met informatie (of vorderingen) over de gebruiker die deze uniek. Standaard bevat deze gebruikersnaam, e-mailadres, voornaam en achternaam van de gebruiker. 

U kunt weergeven of bewerken van de claims die zijn verzonden in de SAML-token aan de toepassing op het tabblad **kenmerken** . 

![][7]

Er zijn twee mogelijke redenen waarom u moet mogelijk de vorderingen in het SAML-token verleend bewerken: •het toepassing is geschreven om een andere set van URI's claim of vordering waarden •Your toepassing is geïmplementeerd op een manier die de NameIdentifier moet beweert te zijn iets anders dan de gebruikersnaam (btw UPN-naam) opgeslagen in Azure Active Directory. 

Bekijk voor meer informatie over het toevoegen en bewerken van vorderingen met betrekking tot deze scenario's in dit [artikel betreffende de aanpassing van de vorderingen](active-directory-saml-claims-customization.md). 

###<a name="testing-the-saml-application"></a>Testen van de SAML-aanvraag 

Zodra de SAML-URL's en het certificaat zijn geconfigureerd in Azure AD en in de toepassing, gebruikers of groepen zijn toegewezen aan de toepassing in Azure, en de claims hebt bestudeerd en zo nodig bewerkt, wordt de gebruiker zich kan aanmelden bij de toepassing. 

Test gewoon sign-in de Azure AD toegang hebt tot het Configuratiescherm op https://myapps.microsoft.com met een gebruikersaccount die u hebt toegewezen aan de toepassing en klik op de tegel voor de toepassing op ere van het proces voor eenmalige aanmelding. Ook kunt u rechtstreeks naar de URL aanmelding voor de toepassing en -in vanaf daar. 

Praktische tips, vindt u in dit [artikel over hoe u foutopsporing op basis van SAML eenmalige aanmelding voor toepassingen](active-directory-saml-debugging.md) 

##<a name="password-single-sign-on"></a>Eenmalige aanmelding wachtwoord

Selecteer deze optie configureren [op basis van wachtwoorden eenmalige aanmelding](active-directory-appssoaccess-whatis.md) voor een webtoepassing die een HTML-aanmeldingspagina. Op basis van wachtwoorden SBF, ook bekend als wachtwoord vaulting, kunt u beheren de toegang van gebruikers en wachtwoorden voor webtoepassingen die geen ondersteuning voor id-federatie bieden. Het is ook handig voor scenario's waarin meerdere gebruikers één account delen, zoals van uw organisatie sociale media app rekeningen moeten. 

Selecteer **volgende**en wordt u gevraagd naar het invoeren van de URL van de pagina van de toepassing op het web-in. Zorg ervoor dat dit de pagina met de gebruikersnaam en het wachtwoord invoervelden. Eenmaal ingevoerd, begint de Azure AD een procedure voor het parseren van de pagina aanmelden voor een invoer gebruikersnaam en een wachtwoord invoeren. Als het proces mislukt, vervolgens begeleidt die u bij een alternatief proces van het installeren van een browserextensie (vereist Internet Explorer, Chrome of Firefox) kunt u de velden handmatig vastleggen.

Zodra de aanmeldingspagina wordt vastgelegd, gebruikers en groepen kunnen worden toegewezen en referentie beleid net als gewone [wachtwoord SSO-apps](active-directory-appssoaccess-whatis.md)kunnen worden ingesteld.

Opmerking: U kunt een logo naast elkaar voor de toepassing met de knop **Logo uploaden** in het tabblad **configureren** voor de toepassing uploaden. 

##<a name="existing-single-sign-on"></a>Bestaande Single Sign-On

Selecteer deze optie om een koppeling toevoegen aan een toepassing als het deelvenster Azure AD Access of Office 365 portal van uw organisatie. U kunt deze koppelingen toevoegen aan de aangepaste web apps die momenteel gebruikmaken van Azure Active Directory Federation Services (of andere federation-service) in plaats van Azure AD voor verificatie. Of u kunt koppelingen diep toevoegen aan specifieke SharePoint-pagina's of andere webpagina's die u wilt op van de gebruiker toegang tot deelvensters worden weergegeven. 

Nadat u hebt **volgende**, wordt u gevraagd aan het invoeren van de URL van de toepassing koppelen aan. Zodra voltooid, kunnen gebruikers en groepen worden toegewezen aan de toepassing, waardoor de toepassing moet worden weergegeven in het [Office 365-app starten](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) of in het [deelvenster Azure AD-toegang](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users) voor gebruikers.

Opmerking: U kunt een logo naast elkaar voor de toepassing met de knop **Logo uploaden** in het tabblad **configureren** voor de toepassing uploaden.

## <a name="related-articles"></a>Verwante artikelen

- [Artikel-Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
- [Claims die worden uitgegeven in het SAML-Token voor vooraf geïntegreerde toepassingen aanpassen](active-directory-saml-claims-customization.md)
- [Het oplossen van problemen op basis van SAML Single Sign-On](active-directory-saml-debugging.md)

<!--Image references-->
[1]: ./media/active-directory-saas-custom-apps/customapp1.png
[2]: ./media/active-directory-saas-custom-apps/customapp2.png
[3]: ./media/active-directory-saas-custom-apps/customapp3.png
[4]: ./media/active-directory-saas-custom-apps/customapp4.png
[5]: ./media/active-directory-saas-custom-apps/customapp5.png
[6]: ./media/active-directory-saas-custom-apps/customapp6.png
[7]: ./media/active-directory-saas-custom-apps/customapp7.png
